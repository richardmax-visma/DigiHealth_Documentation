# Relations

Mermaid diagrams showing class relationships for HelsenorgeAktivSjekken.

- [RequestRelations.mmd](RequestRelations.mmd) – Request to context area mapping

```mermaid
classDiagram
	class Request {
		+string[] fnrListe
		+Omraade omraade
	}

	class Omraade {
		<<enumeration>>
		Helsehjelp
		Ungdom
	}

	Request --> Omraade : omraade

```

- [ResponseRelations.mmd](ResponseRelations.mmd) – Response status composition

```mermaid
classDiagram
	class Response {
		+Map~String, ErAktivStatus~ erAktivListe
	}

	note for Response "erAktivListe key = fnr (national ID)"

	class ErAktivStatus {
		+bool erDigitaltAktivSelv
		+bool erDigitaltAktivViaAndre
		+bool tildeltFullmakt
	}

	Response *-- ErAktivStatus : contains

```
