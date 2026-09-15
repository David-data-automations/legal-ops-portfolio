# Technical Specification: Filevine <-> HubSpot Integration

## 1. Objective
Enable seamless transition of "Qualified Leads" from HubSpot to "Active Matters" in Filevine, ensuring data integrity and reducing manual entry for legal staff.

## 2. System Mapping

### HubSpot Contact/Deal -> Filevine Project
| HubSpot Field | Filevine Field (Schema) | Data Type | Notes |
|---|---|---|---|
| `firstname` | `person.firstName` | String | Primary Contact |
| `lastname` | `person.lastName` | String | |
| `email` | `person.email` | String | Unique Identifier |
| `phone` | `person.phone` | String | |
| `matter_type` | `projectType.name` | Enum | e.g., PI, MVA, Workers Comp |
| `deal_amount` | `settlement.projectedValue` | Decimal | Preliminary forecast |

## 3. Workflow Logic (Power Automate / Zapier / Custom Code)
1. **Trigger:** HubSpot Deal stage updated to "Ready for Legal Review".
2. **Action:**
    *   Search Filevine for existing Contact by Email.
    *   If NOT exists: Create Contact in Filevine.
    *   Create Project in Filevine using `matter_type` to select correct Project Template.
    *   Link Contact to Project as "Client".
3. **Outcome:** New project appears in Filevine "Intake" phase; Lead Attorney assigned automatically via Filevine auto-tasking.

## 4. API Endpoints (v2)
*   **Create Contact:** `POST /core/persons`
*   **Create Project:** `POST /core/projects`
*   **Add Note:** `POST /core/projects/{projectId}/notes` (to log the source HubSpot Deal ID)

## 5. Error Handling
*   **Duplicate Detection:** If contact exists, update record instead of creating new.
*   **Retry Logic:** 3 attempts on 5xx errors; notification to Legal Ops Analyst on 4xx validation errors.
