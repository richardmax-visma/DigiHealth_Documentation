# HelsenorgeAktivSjekken (Digital Activity Check)

Determines whether a citizen is digitally active on Helsenorge and how they can be reached. Use this API before sending digital notices or enrolling citizens into digital flows.

## Authentication

- Requires machine-to-machine (client credentials) via HelseID with scope granting access to HelsenorgeAktivSjekken. See HelseID requirements reference above.
- Include the bearer token in `Authorization: Bearer <token>`.

## Request body

| Field      | Type     | Required | Description                                   |
| ---------- | -------- | -------- | --------------------------------------------- |
| `fnrListe` | string[] | Yes      | List of national ID numbers (max 1000 items). |
| `omraade`  | int      | Yes      | Context area for the check.                   |

Class definitions: `Classes/Request.mmd`, `Classes/Omraade.mmd`.

## Omraade enum

| Value | Name       | Norwegian  | English                             |
| ----- | ---------- | ---------- | ----------------------------------- |
| `3`   | HELSEHJELP | Helsehjelp | Healthcare                          |
| `6`   | UNGDOM     | Ungdom     | Youth (13+, school health services) |

## Response body

| Field          | Type                       | Description                           |
| -------------- | -------------------------- | ------------------------------------- |
| `erAktivListe` | Map<string, ErAktivStatus> | Map of national ID → activity status. |

### ErAktivStatus

| Field             | Type | Norwegian          | English                                                            |
| ----------------- | ---- | ------------------ | ------------------------------------------------------------------ |
| `erAktivSelv`     | bool | Er aktiv selv      | Citizen is digitally active themselves (can be reached directly).  |
| `erAktivViaAndre` | bool | Er aktiv via andre | Citizen is active via another person (representative/guardian).    |
| `tildeltFullmakt` | bool | Tildelt fullmakt   | Citizen has granted a power of attorney or lacks consent capacity. |

Class definitions: `Classes/Response.mmd`, `Classes/ErAktivStatus.mmd`.

## Business rules

- Children under 16 are not `erAktivSelv`; contact must go via guardian/representative.
- `erAktivViaAndre` includes parent representation and registered powers of attorney.
- `tildeltFullmakt` implies the citizen is not competent to consent; communicate via representative.

## Example

**Request**

```http
POST /digitaltaktiv/helsenorgeaktivsjekken/v1 HTTP/1.1
Host: eksternapi.hn.test.nhn.no
Authorization: Bearer <token>
Content-Type: application/json

{
	"fnrListe": ["12345678901", "10987654321"],
	"omraade": 3
}
```

**Response**

```json
{
  "erAktivListe": {
    "12345678901": {
      "erAktivSelv": true,
      "erAktivViaAndre": false,
      "tildeltFullmakt": false
    },
    "10987654321": {
      "erAktivSelv": false,
      "erAktivViaAndre": true,
      "tildeltFullmakt": true
    }
  }
}
```

## Diagrams

- Class relations: see `Relations/ClassRelations.mmd` (Mermaid class diagram).
- Call flow: see `HelsenorgeAktivSjekken_Flow.mmd` (Mermaid sequence diagram).

Use these diagrams alongside the text above; update them when fields or steps change.

## Environments and endpoint

- Test base URL: `https://eksternapi.hn.test.nhn.no`
- Production base URL: `https://eksternapi.helsenorge.no`
- Endpoint: `POST /digitaltaktiv/helsenorgeaktivsjekken/v1`

## References

- HelsenorgeAktiv (general): https://helsenorge.atlassian.net/wiki/spaces/HELSENORGE/pages/1810268162/HelsenorgeAktiv
- HelsenorgeAktivSjekken: https://helsenorge.atlassian.net/wiki/spaces/HELSENORGE/pages/2043478017/HelsenorgeAktivSjekken
- Digital activity Swagger (test): https://eksternapi.hn.test.nhn.no/digitaltaktiv/swagger/index.html
- System-to-system HelseID requirements: https://helsenorge.atlassian.net/wiki/spaces/HELSENORGE/pages/2663776258/Krav+til+bruk+av+HelseID+for+system+til+system+tilgang+til+APIer
- Messaging update requirements: https://helsenorge.atlassian.net/wiki/spaces/HELSENORGE/pages/2904064001/Krav+til+oppdatert+versjon+av+Helsenorge+Messaging
- Message exchange overview: https://helsenorge.atlassian.net/wiki/spaces/HELSENORGE/pages/690913297/Meldingsutveksling+med+Helsenorge
