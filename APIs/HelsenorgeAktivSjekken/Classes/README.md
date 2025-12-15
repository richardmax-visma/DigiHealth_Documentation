# Classes

Domain models for HelsenorgeAktivSjekken requests and responses.

- [Request.mmd](Request.mmd) – Request payload fields

```mermaid
classDiagram
	class Request {
		+string[] fnrListe
		+Omraade omraade
	}

```

- [Omraade.mmd](Omraade.mmd) – Context area enum

```mermaid
classDiagram
	class Omraade {
		<<enumeration>>
		Helsehjelp
		Ungdom
	}

	note for Omraade "Volven 7618: Helsehjelp=3, Ungdom=6"

```

- [Response.mmd](Response.mmd) – Response wrapper with status map

```mermaid
classDiagram
	class Response {
		+Map~String, ErAktivStatus~ erAktivListe
	}

	note for Response "erAktivListe key = fnr (national ID)"

```

- [ErAktivStatus.mmd](ErAktivStatus.mmd) – Activity status flags

```mermaid
classDiagram
	class ErAktivStatus {
		+bool erDigitaltAktivSelv
		+bool erDigitaltAktivViaAndre
		+bool tildeltFullmakt
	}

```
