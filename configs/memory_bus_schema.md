# Appendix D. Shared Memory Bus Schema
To ensure traceability and auditability, the Shared Memory Bus is implemented as a relational SQLite database rather than a vector store. This schema guarantees that the exact Chain-of-Thought is preserved verbatim.

```sql
CREATE TABLE batches (
                batch_id INTEGER PRIMARY KEY AUTOINCREMENT,
                dataset_name TEXT NOT NULL,
                domain TEXT,
                sample_size INTEGER,
                timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
                categories_used TEXT,
                model_name TEXT,
                status TEXT NOT NULL CHECK(status IN (
                    'ate_in_progress', 'ate_complete', 'failed',
                    'review_in_progress', 'review_complete',
                    'revision_in_progress', 'revision_complete',
                    'validation_in_progress', 'validation_complete'
                )),
                validation_metrics_json TEXT,
                validation_summary_text TEXT
            , initial_metrics_json TEXT, parent_batch_id INTEGER);
CREATE TABLE sqlite_sequence(name,seq);
CREATE TABLE results (
                result_id INTEGER PRIMARY KEY AUTOINCREMENT,
                batch_id INTEGER NOT NULL,
                original_review_id TEXT,
                review_text TEXT NOT NULL,
                actual_aspects TEXT,
                ate_extracted_aspects TEXT,
                final_revised_aspects TEXT,
                FOREIGN KEY (batch_id) REFERENCES batches (batch_id)
            );
CREATE TABLE run_logs (
                log_id INTEGER PRIMARY KEY AUTOINCREMENT,
                result_id INTEGER NOT NULL,
                stage_name TEXT NOT NULL,
                raw_output_json TEXT,
                error_message TEXT,
                timestamp DATETIME DEFAULT CURRENT_TIMESTAMP, error_code TEXT, error_details TEXT,
                FOREIGN KEY (result_id) REFERENCES results (result_id)
            );
CREATE TABLE escalation_cases (
                escalation_id INTEGER PRIMARY KEY AUTOINCREMENT,
                batch_id INTEGER NOT NULL,
                experiment_id TEXT NOT NULL,
                problematic_aspect_category TEXT,
                trigger_reason TEXT NOT NULL,
                trigger_context_json TEXT NOT NULL,
                status TEXT NOT NULL CHECK(status IN (
                    'pending_escalation', 'in_progress', 'diagnosis_pending_review',
                    'diagnosis_approved', 'resolved', 'failed_max_iterations', 'failed'
                )),
                timestamp DATETIME DEFAULT CURRENT_TIMESTAMP, agent_findings TEXT, last_updated_timestamp DATETIME,
                FOREIGN KEY (batch_id) REFERENCES batches (batch_id)
            );
CREATE TABLE diagnoses (
                diagnosis_id INTEGER PRIMARY KEY AUTOINCREMENT,
                escalation_id INTEGER NOT NULL,
                status TEXT NOT NULL CHECK(status IN ('pending_review', 'approved')),
                diagnosis_text TEXT NOT NULL,
                timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
                FOREIGN KEY (escalation_id) REFERENCES escalation_cases (escalation_id)
            );
CREATE TABLE execution_plans (
                plan_id INTEGER PRIMARY KEY AUTOINCREMENT,
                escalation_id INTEGER NOT NULL,
                plan_text TEXT NOT NULL,
                execution_log_json TEXT,
                timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
                FOREIGN KEY (escalation_id) REFERENCES escalation_cases (escalation_id)
            );
CREATE TABLE art_proposals (
                proposal_id INTEGER PRIMARY KEY AUTOINCREMENT,
                escalation_id INTEGER NOT NULL,
                proposal_text TEXT NOT NULL,
                timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
                FOREIGN KEY (escalation_id) REFERENCES escalation_cases (escalation_id)
            );
CREATE TABLE agent_run_steps (
                step_id INTEGER PRIMARY KEY AUTOINCREMENT,
                fk_escalation_id INTEGER NOT NULL,
                timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
                iteration_num INTEGER NOT NULL,
                art_invoked TEXT NOT NULL,
                art_input TEXT,
                art_output TEXT,
                agent_reflection TEXT,
                FOREIGN KEY(fk_escalation_id) REFERENCES escalation_cases(escalation_id)
            );
CREATE TABLE case_summaries (
                summary_id INTEGER PRIMARY KEY AUTOINCREMENT,
                escalation_id INTEGER NOT NULL UNIQUE,
                summary_json TEXT NOT NULL,
                timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
                FOREIGN KEY (escalation_id) REFERENCES escalation_cases (escalation_id)
            );
```
