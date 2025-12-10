# Medlemstjenester (Membership Services)

**Technology:** FHIR (v4.0)  
**Endpoint:** `https://eksternapi.helsenorge.no/helsekontakter/api/v1`  
**Auth Scope:** Helsekontakter

## When to use

- You offer the same services to all members and need them visible in Helsenorge.
- You must register member lists so citizens can use digital services (dialog, appointments).

## Authentication

Two options:

1. **HelseID** – scope "Helsekontakter" in self-service
2. **Helsenorge STS** – pre-configure with public key

## Process (2 steps)

### 1) Send health offerings (Helsetilbud)

Defines what services members receive.

Core classes: [HealthcareService](Classes/HealthcareService.mmd), [Organization](Classes/Organization.mmd), [Location](Classes/Location.mmd), [Endpoint](Classes/Endpoint.mmd), [Address](Classes/Address.mmd), [Telecom](Classes/Telecom.mmd).

| Resource              | Field               | Description                     | Required |
| --------------------- | ------------------- | ------------------------------- | -------- |
| **HealthcareService** | identifier          | Local GUID                      | ✅       |
|                       | active              | Is active                       |          |
|                       | name                | Service name (shown to citizen) | ✅       |
|                       | comment             | Description                     |          |
|                       | providedBy          | Ref to Organization             | ✅       |
|                       | endpoint.identifier | Communication partner ID        | ✅       |
| **Organization**      | identifier          | Org number (e.g., 948 554 062)  | ✅       |
|                       | name                | Organization name               | ✅       |
|                       | endpoint.id         | Business ID                     | ✅       |
| **Location**          | address.line        | Street address                  | ✅       |
|                       | address.city        | City                            | ✅       |
|                       | address.postalCode  | Postal code                     | ✅       |
|                       | telecom.value       | Phone/URL                       |          |

### 2) Send member list (Medlemsliste)

Links members to offerings.

Classes: [Patient](Classes/Patient.mmd), [Contact](Classes/Contact.mmd), [Period](Classes/Period.mmd).

| Field                           | Description                          | Required |
| ------------------------------- | ------------------------------------ | -------- |
| identifier                      | National ID (fødselsnummer/d-nummer) | ✅       |
| contact.organization.identifier | Organization number                  | ✅       |
| contact.period.start            | Service start date                   | ✅       |
| contact.period.end              | Service end date                     | ✅       |

## Diagrams

- Flow: [Medlemstjenester_Flow.mmd](Medlemstjenester_Flow.mmd)
- HealthcareService relations: [Relations/HealthcareServiceRelations.mmd](Relations/HealthcareServiceRelations.mmd)
- Patient relations: [Relations/PatientRelations.mmd](Relations/PatientRelations.mmd)
- Classes: [Classes folder](Classes/)

## Non-functional requirements

| Type                      | Requirement                          |
| ------------------------- | ------------------------------------ |
| Response time (offerings) | < 3 sec for 99%                      |
| Response time (members)   | < 10 sec for 99%                     |
| Timeout (members)         | 15 sec                               |
| Retry                     | Must handle resending if no response |

## Environments

| Environment | URL                                                              |
| ----------- | ---------------------------------------------------------------- |
| MAS-01      | https://eksternapi-hn-mas-01.int-hn.nhn.no/helsekontakter/api/v1 |
| MAS-02      | https://eksternapi-hn-mas-02.int-hn.nhn.no/helsekontakter/api/v1 |
| TEST1       | https://eksternapi.hn.test.nhn.no/helsekontakter/api/v1          |
| TEST2       | https://eksternapi.hn2.test.nhn.no/helsekontakter/api/v1         |
| PROD        | https://eksternapi.helsenorge.no/helsekontakter/api/v1           |

## Notes

- If citizen hasn't consented to Helsenorge, their info cannot be stored.
- Track digital status and periodically retry inactive members.
- Split member lists into batches to avoid timeouts.

## Sources

- Medlemstjenester (official): https://helsenorge.atlassian.net/wiki/spaces/HELSENORGE/pages/24018962/Medlemstjenester
