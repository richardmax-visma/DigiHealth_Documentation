# Helsekontakt (Health Contact)

Helsekontakt is the citizen's entry point for digital communication with healthcare providers via Helsenorge. It enables dialog and appointment management.

## Health Contact Types

1. **Fastlege** - General Practitioner (from FLO registry)
2. **Hjemmetjenester** - Home care services
3. **Øvrig primærhelsetjeneste** - Other primary care
4. **Spesialisthelsetjeneste** - Specialist care
5. **Medlemstjenester** - Membership services

## Technical Implementations

| Technology  | API                                                        | Use Case                | Status                |
| ----------- | ---------------------------------------------------------- | ----------------------- | --------------------- |
| AMQP        | [Tjenesteoversikt](./AMQP_Tjenesteoversikt_README.md)      | Home care services only | In Production         |
| FHIR        | [Medlemstjenester](./Medlemstjenester_README.md)           | Membership services     | In Production         |
| AMQP + FHIR | [Notifikasjon Helsekontakt](./AMQP_Notifikasjon_README.md) | General notifications   | In Production (Pilot) |

## Diagrams

### AMQP Tjenesteoversikt

- `AMQP_Tjenesteoversikt_Flow.mmd` - Message flow
- `AMQP_Tjenesteoversikt_Classes.mmd` - Message structure

### Medlemstjenester (FHIR)

- `Medlemstjenester_Flow.mmd` - API flow
- `Medlemstjenester_Classes.mmd` - FHIR resources

### AMQP Notifikasjon Helsekontakt

- `AMQP_Notifikasjon_Flow.mmd` - Message flow
- `AMQP_Notifikasjon_Classes.mmd` - FHIR EpisodeOfCare structure

## Quick Comparison

| Feature    | Tjenesteoversikt | Medlemstjenester | Notifikasjon   |
| ---------- | ---------------- | ---------------- | -------------- |
| Technology | AMQP             | REST/FHIR        | AMQP + FHIR    |
| Use case   | Home care        | Group services   | General        |
| Auth       | AMQP certs       | HelseId/STS      | AMQP certs     |
| Payload    | XML (MsgHead)    | FHIR Bundle      | MsgHead + FHIR |
