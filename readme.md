# Peppol-utils 1.0.0 End-User Guide

## Table Of Contents

- [What's New In 1.0.0](#whats-new-in-100)

**Getting started**

1. [What You Receive](#1-what-you-receive)
2. [Install Java](#2-install-java)
3. [Start The App](#3-start-the-app)
4. [Open An Invoice](#4-open-an-invoice)

**Using the app**

5. [Read The Summary](#5-read-the-summary)
6. [View A PDF Invoice](#6-view-a-pdf-invoice)
7. [Edit XML](#7-edit-xml)
8. [Check Peppol Participant Information](#8-check-peppol-participant-information)
- [Use The Knowledge Base](#use-the-knowledge-base)
9. [Validate The Invoice](#9-validate-the-invoice)
10. [Visualize Edited XML](#10-visualize-edited-xml)
11. [Convert Between UBL And CII](#11-convert-between-ubl-and-cii)
12. [Generate France CTC Flow 1 XML](#12-generate-france-ctc-flow-1-xml)
13. [Save Changes](#13-save-changes)

**Help and safe use**

14. [Troubleshooting](#14-troubleshooting)
15. [Safe Use Recommendation](#15-safe-use-recommendation)

## What's New In 1.0.0

This guide applies to the `peppol-utils-1.0.0.jar` desktop app. Version 1.0.0 internally decomposes the large `FacturxService` implementation into focused components while preserving the public `FacturxService` facade, reusable `com.facturxutils` API, invoice workflows, validation semantics, and save guarantees. It retains the low-resource improvements delivered through 0.9.0: bounded startup and working memory, viewport-sized PDF rendering, AI Wiki resource unloading, and short-lived workers for validation, conversion, France CTC Flow 1 generation, and XML-to-PDF visualization.

- Continue using the same supported invoice workflows and API-compatible integrations after the internal service decomposition.
- Choose `Save anyway` for a Factur-X PDF when edited XML is well-formed but has schema, profile, or business-rule findings; the rewritten PDF still has to pass Factur-X container and PDF/A-3 checks.
- Read complete validation and save-failure details in resizable, scrollable dialogs, including rule IDs, locations, artefacts, failed tests, and validator roles.
- Repair supported Factur-X XMP/output-intent and attachment metadata defects while re-embedding XML. The safe staging repair can remove stale Factur-X attachment references, remove an empty AcroForm with no widgets, and embed mapped Standard-14 Helvetica/Times-family font substitutions, including nested form resources.

- Start in a centered, right-sized window instead of allocating a maximized graphics surface immediately.
- Preview PDFs with a bounded viewport-aware renderer designed for integrated GPUs and low-memory computers.
- Load invoice validation, visualization, AI providers, and Knowledge Base data only when their workflows need them.
- Disable AI Wiki from its settings to release its local index, provider clients, conversations, and rendered messages.
- Edit large XML documents without applying expensive whole-document syntax highlighting above the safe interactive limit.

- Search a signed, versioned e-invoicing documentation bundle locally from `Utilities` > `AI Wiki`.
- Ask questions through local Ollama or optional Gemini and OpenAI providers, and inspect application-generated source cards.
- Continue using local source search when the selected provider is unavailable. The current question may be sent alone to the selected provider for bounded search planning before local retrieval; questions with insufficient evidence do not trigger an answer request.
- Convert UBL Invoice/CreditNote to CII and CII/Factur-X XML to UBL from `Conversion`.
- Generate Base or Full France CTC Flow 1 XML while retaining CII or UBL syntax.
- Validate Flux 1 and Flux 10 with the Helger phive rules selected from the Flux 1 BT-24/CustomizationID trajectory or the Flux 10 `<Report>` root.
- Check Peppol participant publication and exact invoice-type support separately in Production and Test.
- Use the dedicated two-layer France CTC validation route for Flux 2/CDAR documents.
- Review, edit, and save converted or generated XML in separate tabs without overwriting the source invoice.

## 1. What You Receive

The universal JAR contains the app plus the libraries it needs for PDF rendering, Factur-X extraction, XML editing, validation rules, UBL/CII conversion in both directions, and JavaFX controls. It supports Windows x64, Linux x64/ARM64, and macOS Intel/Apple Silicon. A self-contained Windows installer may also be supplied; it installs the launcher, JavaFX/native libraries, resources, and its own private Java runtime.

Users do not need Maven or the source code.

## 2. Install Java

When using the JAR, Peppol-utils requires Java 21. The Windows installer already includes the required runtime, so Windows users do not need a separate Java, JRE, or JDK installation for that package.

Install a Java 21 runtime or JDK from a vendor you trust. JavaFX is bundled with the application; the universal JAR selects the matching native files for its supported operating system and CPU architecture.

Recommended download page:
- Oracle Java downloads: https://www.oracle.com/java/technologies/downloads/

## 3. Start The App

For the self-contained Windows package, download and run
`Peppol-utils-<version>-windows-x64-setup.exe`. Complete the installer, then
launch Peppol-utils from the Start menu or the optional desktop shortcut.

For the universal JAR, put the jar in a folder you can find, for example
`Downloads` or `Documents`. Open PowerShell in that folder and run:
```powershell
java -jar .\peppol-utils-1.0.0.jar
```

## 4. Open An Invoice

1. Click `Open invoice`.
2. Choose one of these file types:
   - `.pdf` for a Factur-X PDF invoice
   - `.xml` for a standalone invoice using CII or UBL syntax
3. Wait for the invoice to load.

For a PDF invoice, the app shows:

- `Summary`: main invoice fields such as invoice number, seller, buyer, totals, profile, and VESID.
- `PDF`: a rendered preview of the PDF pages.
- `XML`: the embedded Factur-X XML that can be edited.
- `Conversion`: UBL-to-CII, CII-to-UBL, and XML-to-PDF actions for the current editor content.
- `Visualize invoice`: a generated invoice preview from the XML.
- `Validation`: validation messages and warnings.

For a standalone XML invoice, the PDF tab is disabled and the XML/summary/validation views are used.

## 5. Read The Summary

Open the `Summary` tab to quickly inspect the invoice:

- Invoice number
- Type code
- Issue date
- Seller and buyer
- Currency
- Line count
- Tax total
- Grand total
- Due payable
- Container format, XML syntax, guideline, profile, and VESID
- Seller and buyer Peppol electronic-address information

For a hybrid PDF, the format is detected from Factur-X/ZUGFeRD XMP metadata and the standard embedded XML filename. This distinction is important for EN 16931 because its BT-24 value can be identical in generic CII and Factur-X. If the XML is edited and still well-formed, the summary updates while retaining the opened PDF's container context.

## 6. View A PDF Invoice

Open the `PDF` tab.

Use:

- `<` and `>` to move between pages.
- `-` and `+` to zoom out and in.
- The scrollbars to move around the page when it is larger than the view.

The PDF page is centered in the view with padding around it. The PDF tab is only available when the opened invoice is a PDF.

## 7. Edit XML

Open the `XML` tab.

Available actions:

- Edit the XML directly in the editor.
- Click `Format XML` to pretty-print the XML.
- Use the `-` and `+` controls to adjust XML editor text size from 75% to 200%; generated and converted XML tabs provide the same controls.
- Press `Ctrl+F` to search in the XML.
- Use `Previous`, `Next`, and `Close` in the search bar.
- Click `Visualize` to generate a PDF preview from the current XML.
- Click `Save as...` to write the current invoice to a new PDF or XML file without first choosing the overwrite/save-copy dialog.
- Use the editor scrollbars to move vertically or horizontally through long XML files.

When you edit the XML, the app marks the invoice as `Unsaved`.

## 8. Check Peppol Participant Information

The `Summary` tab shows a `Peppol participant information` area for the seller and buyer. The app reads only the standard electronic-address fields from the invoice and first checks locally whether each address has an eligible Peppol participant scheme. It does not substitute a tax ID, legal ID, generic party ID, or email address.

To check publication status:

1. Open the `Summary` tab.
2. Review the detected seller and buyer electronic addresses and schemes.
3. Click `Check Participant Information`.
4. Review the separate `Production` and `Test` results.
5. Review the invoice-type badge in each Production and Test row, then expand a result to inspect the discovery details. These can include the SML DNS zone, SMP URL, a selected Billing Invoice access point (active when available, otherwise the earliest future or another inactive endpoint), Document Type details, and optional Business Card company names, country, and registration date when the SMP provides them. Document Type details are collapsed by default; expand one to see its blue bold name, scheme and identifier/value, plus each matching process pair in its own card with service activation, expiry, and lifecycle status. The collapsed `SMP` header shows the SMP URL; expand it to inspect the signing certificate details embedded in validated SMP XML signatures (subject, issuer, serial, validity, key, and certificate signature algorithm) when available. This is distinct from both the SMP server TLS certificate and the AP endpoint certificate. The collapsed `Access Point` header shows the selected endpoint URL and lifecycle status; expand it under `SMP & AP` to inspect the transport, URL, service, contact, certificate, activation, expiry, and status metadata once for the lookup result when the SMP supplies it. A `Not yet active` endpoint is published metadata, not a currently usable route. The certificate fields describe the AP endpoint certificate published in SMP service metadata; the SMP server TLS certificate is not captured. Click `Refresh` to bypass the short-lived cache and run the checks again.

Production and Test are checked independently. The Peppol overview reports two separate conclusions: document-type publication (`published by SMP`, `not published by SMP`, or `publication unknown`) and advertised process/endpoint metadata (`active AS4 v2 endpoint metadata available`, `active endpoint metadata available; choose a process`, `no matching active endpoint metadata`, or `process/endpoint unresolved`). Document publication uses the exact Peppol document type identifier first (case-sensitive scheme and value), then the standard Peppol Wildcard Scheme. An exact same-scheme/value registration takes precedence; otherwise the selector chooses the wildcard with the greatest number of matching CustomizationID parts. A published document type is not by itself proof that a requested process can be delivered; an active metadata row is not an AS4 reachability test.

For a recognized Factur-X hybrid PDF with explicit France CTC buyer routing (`schemeID=0225`), the comparison uses the French `pdf+xml` Factur-X transport type, not the embedded CII XML type. The embedded XML remains the source for invoice data and validation. A recognized Factur-X PDF without that routing context remains unresolved; the embedded CII type is never used as a fallback. Standalone XML carries its raw DDD BT-23/ProfileID process into the comparison, and a value that is not an exact member of the matched DTI's allowed process set is rejected. A Factur-X PDF's embedded BusinessProcess value is not treated as its Peppol transport process, so only that PDF case leaves process selection unresolved and exposes the allowed active processes advertised for the matched PDF registration instead. Test support does not imply Production support.

![Peppol participant information showing separate Production and Test results, discovery details, Business Card data, Document Type details, and collapsed SMP signing-certificate and Access Point panels.](docs/images/peppol-participant-information.png)

*Peppol participant information in the Summary tab. Production and Test are reported independently; this example shows a participant registered in Test but not in Production, with the invoice type unsupported in both environments.*

Registration and invoice-type support are separate results. A participant can be registered while not publishing the exact document type required by the open invoice. Possible registration results are registered, not registered, and lookup failed. A missing address, missing or invalid `schemeID`, email address, or non-registrable scheme is explained without sending a lookup.

Peppol lookup requires internet access. It queries Peppol SML/SMP discovery services only when you explicitly run a lookup; opening and validating an invoice do not automatically contact the network. Knowledge Base has a separate, explicit network flow described below. A Peppol result describes discovery metadata at the time of the check and is not a guarantee that a future document transmission will succeed.

### Look up any Peppol participant

Use `Utilities` > `Peppol lookup` in the left sidebar when the participant is not taken from the open invoice.

1. Enter an identifier such as `0225:152857363`. The full `iso6523-actorid-upis::0225:152857363` form is also accepted.
2. Click `Look up participant` or press Enter.
3. Review the separate Production and Test results below the search box.
4. Expand the environment details to inspect its SML/SMP source, the collapsed SMP panel with signing certificate details from validated XML signatures, Document Type details, the collapsed Access Point panel with the selected Billing Invoice endpoint URL, lifecycle status, and metadata, and optional Business Card. SMP signing certificates, AP endpoint certificates, and the SMP server TLS certificate are distinct; the TLS peer certificate is not captured.

The identifier is checked locally before any request is sent. Each lookup is user initiated, Production and Test stay independent, and a network failure remains distinct from a participant that is not registered.

## Use The AI Wiki

Open `Utilities` > `AI Wiki`. The feature is disabled by default and performs no bundle download, update check, or provider request until you opt in.

On first use:

1. Read the disclosure.
2. Select the acknowledgement.
3. Click `Enable and download`.
4. Wait while the signed manifest, checksum, archive paths, schemas, and local search index are verified.

The app activates a new bundle only after a complete staged validation. A failed or untrusted update does not replace the last-known-good bundle. An authenticated revocation immediately removes the affected bundle from use and activates the newest healthy, non-revoked rollback when one is available.

The main area is the AI Wiki chat. The conversation and composer share a centered reading column so long answers remain readable on wide displays. Click `Settings` to update the bundle and choose Ollama local, Gemini, or OpenAI.

You can search locally even when the selected provider is unavailable. Questions in English, French, or Vietnamese use the signed bundle's BM25 index and a curated e-invoicing terminology catalog. When a configured provider is available, it may prepare bounded English, French, and keyword search variants from the current question alone before local retrieval. Follow-up retrieval also uses the preceding user question locally to resolve the referenced standard or concept while the current question remains the one answered. For a question that asks for a list, count, or document-wide explanation, the app chooses the most relevant current source and supplies contiguous source pages rather than disconnected snippets. If no result meets the relevance threshold, the app reports insufficient evidence and does not request an answer from the provider.

For a synthesized answer:

1. For private local generation, start Ollama and install a chat model. For cloud generation, obtain your own Gemini or OpenAI API key.
2. Open `Settings` and choose a provider. Ollama connects only to `http://127.0.0.1:11434/api/`; cloud providers use their official HTTPS APIs.
3. For Ollama, select an installed model. For a cloud provider, enter the exact model name, then save the settings and API key or provide the corresponding environment variable. Each cloud provider remembers its own model; `Use default` restores the bundled default name.
4. Optionally click `Test connection`; the settings dialog confirms the connection or shows a visible error.
5. Type a question and press Enter or click `Send`. Press Shift+Enter to insert a new line. An animated `Assistant is thinking...` indicator remains visible until the first response text arrives.
6. Read the formatted Markdown answer, then click its source action to open the contextual source rail on the right. Sources explicitly cited as `[S1]`, `[S2]`, and so on are grouped by document with consolidated page and marker lists; repository paths and exact commits remain under `Source details`. If the provider returned no valid citation marker, the fallback source action is labeled `Retrieved sources supplied to Assistant`; local-search-only results use `Retrieved sources`.

Hover over a message to reveal its small copy icon inside the upper-right corner. `Chats` opens a session-history dialog; `New chat` keeps the current conversation available while starting a clean one. This lightweight chat history lasts for the current app session and is not written to disk.

The selected provider and cloud model names are remembered. Cloud API keys may be supplied through environment variables or saved in `~/.peppol-utils/knowledgebase.json`; the file is written atomically with owner-only POSIX permissions where supported, but its contents are not encrypted. Prefer environment variables or an operating-system credential workflow on managed devices.

For search planning, the selected provider receives only the current question. If the evidence is sufficient and you request a synthesized answer, the provider receives the current question, at most six recent turns, and selected source excerpts. The app does not attach an invoice or send invoice XML/PDF content. `Stop` cancels an active response, and normal invoice features remain available if GitHub or the provider is offline.

AI output is explanatory, not authoritative validation, legal advice, or tax advice. Follow the immutable source link and check the cited version when accuracy matters.

## 9. Validate The Invoice

Choose a validation profile in the left sidebar:

- `Default`: general Factur-X / EN 16931 validation.
- `France CTC`: Flux 1 and Flux 10 use the Helger phive rule sets automatically. Flux 1 CII/UBL selects the Base/Démarrage or Full/Cible VESID from BT-24/CustomizationID, while the namespace-less Flux 10 `<Report>` selects `fr.ctc.flux10:report:1.0`. Other documents first validate the exact detected invoice schema/profile VESID, then run the legacy 1.3.1 and latest bundled France CTC rule sets for CII/UBL EN 16931, CII/UBL Extended, UBL CreditNote, Factur-X, or CDAR. The legacy set is skipped if it is not bundled.

Click `Validate`.

For a saved Factur-X PDF, the default validation covers the complete hybrid document: Kaltblut checks the Factur-X/ZUGFeRD BR-HYBRID container and XMP rules, veraPDF checks PDF/A-3 conformance, and DDD/phive check the embedded XML schema and business rules. PDF/A findings include the veraPDF rule and PDF object location. Container and XMP checks also run when `France CTC` is selected, alongside that profile's XML validation route.

Before validation is run, the `Validation` tab says `Not validated yet`; opening an invoice does not imply that it passed. A `Compliance & format` strip repeats the detected format, XML syntax, profile, guideline, and VESID from Summary for quick reference before validation.

After validation, the tab shows an overall result and error, warning, and check/information counts. The headline identifies the validation set responsible for a failure when only one set has blocking errors. Results are grouped into user-facing sets such as invoice profile rules, France CTC, and the Factur-X PDF container; engine names such as PHIVE and Kaltblut remain available in technical details.

The table shows actionable errors and warnings, while passed checks and profile-detection messages remain summarized above. Select an issue to inspect its rule ID, description, element XPath, and expected or allowed values when the validator references a bundled code list. Expand `Technical details` for the validator test, engine/layer, rule artefact, source document, and Schematron role when supplied. The technical validator test is the XPath/Schematron condition evaluated by the validation engine; it is diagnostic logic, not usually a value to paste into the invoice.

- Severity
- Validation set
- Rule code
- Message
- Element/context, XPath test, source, and artefact in the detail panel

If there are no validation issues, the table reports an OK result. The validation table labels detected schema/profile findings as `PHIVE`, France CTC 1.3.1 findings as `FR CTC old`, and findings from the latest bundled France CTC version as `FR CTC`. Both France CTC versions run when 1.3.1 is available in Helger phive; if a future dependency release removes it, only the latest set runs. Warnings remain non-blocking; validator execution failures and error/fatal assertions block strict saving.

Flux 1 trajectory selection uses `urn.cpro.gouv.fr:1p0:einvoicingextract#Base` for Démarrage and `...#Full` for Cible. The service reports the corresponding `fr.ctc.flux1:*:0.2` VESID in the ordinary PHIVE validation set. Generated Flow 1 output still applies the field catalog and temporary operational mapping before running the same phive-backed completeness check.

![France CTC validation results showing an error and warning for the same rule in the current and legacy rule sets.](docs/images/france-ctc-validation-results.png)

*France CTC validation result. Select a row to see the rule ID, description, element/context, and technical details in the panel on the right. The same assertion can appear once for the current France CTC rules and once for the legacy `FR CTC old` rules.*

If the XML editor contains unsaved changes, validation checks the edited XML because those bytes are not yet embedded in the saved PDF. Save and reopen the PDF to validate the updated hybrid container and XMP metadata.

## 10. Visualize Edited XML

Open the `XML` tab and click `Visualize`.

The app generates a PDF preview from the current XML and opens the `Visualize invoice` tab.

Use:

- `<` and `>` to move between generated preview pages.
- `-` and `+` to zoom out and in.
- `Save as...` to export the generated PDF visualization.
- The scrollbars to move around pages larger than the view.

This preview helps you inspect how the XML invoice content could look as a readable document.

## 11. Convert Between UBL And CII

Select `Conversion` in the left sidebar. The screen uses the current XML editor content, including unsaved edits.

- Click `UBL to CII` for a UBL 2.1 Invoice or CreditNote. The app opens a `Converted CII` tab.
- Click `CII to UBL` for CII / Factur-X XML. The app opens a `Converted UBL` tab.
- Click `XML to PDF` to open a generated preview in the `Visualize invoice` tab.
- Click `FR CTC F1` to generate a France CTC Flow 1 XML document from supported CII or UBL input.

Each syntax-conversion button is enabled only when the current XML has the matching input syntax. CII input can be:

- A standalone `.xml` file using CII syntax.
- A Factur-X PDF whose embedded invoice XML is CII.

UBL input is a standalone `.xml` UBL Invoice or CreditNote. The original invoice is not overwritten.

In the result tab, you can:

- Edit or format the generated XML.
- Click `Save as...` and choose where to write the CII or UBL XML file.

The suggested output names follow this pattern:

- UBL `invoice.xml` becomes `invoice.cii.xml`.
- UBL `invoice.ubl.xml` becomes `invoice.cii.xml`.
- CII `invoice.xml` becomes `invoice.ubl.xml`.
- CII `invoice.pdf` becomes `invoice.ubl.xml`.

In these names, `cii` or `ubl` labels the generated XML syntax; the file extension remains `.xml`.

For France CTC compatibility, the app also cleans up converted UBL identifiers that have no `schemeID` in contexts where France CTC requires one. The app does not invent a scheme. Properly schemed identifiers are kept.

## 12. Generate France CTC Flow 1 XML

Open any supported CII or UBL invoice, then select `Conversion` and click `FR CTC F1`.

1. Choose `Base` or `Full`. `Base` is selected by default.
2. Click `Generate`.
3. Review the generated `France CTC F1 Base (...)` or `France CTC F1 Full (...)` tab.
4. Use `Format XML` or edit the XML if needed.
5. Click `Save as...` and choose an output location.

The generator keeps the source syntax: CII input produces CII F1 XML, while UBL invoice or credit-note input produces UBL F1 XML. It follows the XP Z12-012 v1.4 field subset and the bundled mapping resources. Ordinary invoices open one generated tab. B8/S8/M8 multi-seller invoices open one tab per unit-invoice number, and B9/S9/M9 bidirectional invoices open the two required directions. The generated result is validated by the matching dependency-supplied phive VESID selected from its BT-24/CustomizationID trajectory; review any reported required-field findings before saving. The app does not claim a separate bundled AIFE Schematron validation route.

The source invoice is not overwritten. The suggested filename is prefixed with the profile; split outputs also include the unit-invoice identifier.

## 13. Save Changes

Click `Save changes`.

The app shows the complete source path and lets you either overwrite the opened invoice or use `Save a copy...`. Saving a copy stages the new file before replacing the selected destination, so the original invoice remains unchanged.

Important:

- The app does not create a backup copy.
- Make your own copy of the invoice before editing if you need to keep the original.
- For standalone XML files, the app validates the edited XML before overwriting the XML file.
- For PDF files, the app validates the edited XML, re-embeds it into the PDF, validates the rewritten PDF, and then replaces the original PDF.
- If validation finds blocking `ERROR` or `FATAL` findings, the app shows the complete rule IDs, messages, locations, and available validator details in a resizable, scrollable dialog and asks whether you want to save the invalid XML anyway. For a Factur-X PDF, `Save anyway` permits XML schema, profile, or business-rule findings, but the embedded XML must still be readable and the rewritten PDF must pass Factur-X BR-HYBRID/XMP, attachment, and PDF/A-3 checks. For a standalone XML file, the explicit low-level save option retains its historical ability to write invalid XML. Choose this only when you intentionally need an XML-invalid file for testing.
- PDF re-embedding uses Mustangproject's `ZUGFeRDExporterFromA3`. When the source has damaged or missing hybrid metadata such as XMP or output intents, the save path may rebuild that container metadata from a sanitized PDFBox staging copy. That staging repair removes only Factur-X attachment references, preserves unrelated name trees and associated files, can remove an empty AcroForm only when it has no fields or widget annotations, and can recursively replace unembedded Standard-14 Helvetica/Times-family fonts with embedded TrueType substitutions. Supported substitutions retain widths and any existing ToUnicode map where possible, but may normalize a non-PDF/A source encoding to WinAnsi; the final Kaltblut and veraPDF checks remain mandatory, including for PDF/A-3U Unicode requirements. It does not flatten live forms, remove widgets or signature fields, or replace arbitrary/unmappable fonts. A save error also opens its full available details in the same scrollable dialog.
- If a normal save—or a save after you choose `Save anyway` for XML business-rule findings—fails because a preserved PDF digital signature makes the rewritten PDF/A container invalid, the app offers an explicit unsigned test-copy recovery. The suggested output is `<stem>-invalid.pdf`; it removes the PDF signature fields/widgets and signature permissions only from that new copy, never from or over the original signed invoice. The recovery still requires well-formed, XML-schema-valid XML and a valid Factur-X/PDF-A container; only the already-confirmed business-rule (Schematron) bypass is retained.

After saving, the app reopens the saved file and shows the validation result.

### Repair an invalid Factur-X PDF container

When a PDF's embedded invoice XML is valid but its Factur-X XMP, PDF/A-3 metadata, checksum, output intent, or embedded-file metadata is invalid, open the `Validation` tab and click `Repair PDF container...`.

The repair action preserves the saved embedded XML, re-exports the hybrid container through Mustangproject, and validates the staged result before writing it. It always creates a separate `-repaired.pdf` copy by default; the original invoice is not overwritten. Save or discard any XML editor changes before starting repair.

Repair can rebuild hybrid metadata and attachment structures. It does not promise to correct arbitrary damaged page content, encryption, signatures, malformed PDFs, or every possible PDF/A violation. If the repaired copy does not pass validation, the app does not replace the selected destination.

## 14. Troubleshooting

### `java` is not recognized

Java is not installed or the terminal was opened before Java was installed.

Fix:

1. Install Java 21.
2. Close PowerShell or Command Prompt.
3. Open a new PowerShell or Command Prompt window.
4. Run:

```powershell
java -version
```

### The app does not start

Run it from PowerShell so you can see the error:

```powershell
java -jar .\peppol-utils-1.0.0.jar
```

Check that:

- Java version is `21`.
- The jar file was fully downloaded.
- The jar was built for your operating system and CPU architecture.

### macOS JavaFX or `QuantumRenderer` crash

If the app crashes at startup and the Terminal output mentions JavaFX, native libraries, `QuantumRenderer`, or the graphics pipeline, the most likely cause is a mismatch between the jar's bundled JavaFX files and the Mac's architecture.

Check the Mac architecture:

```sh
uname -m
```

The released universal JAR contains both macOS native payloads. Keep using that
same JAR, and install a Java 21 runtime whose architecture matches the Mac:

- `arm64`: Apple Silicon / M-series Mac, including M1, M2, M3, M4, and M5. Use an arm64 Java 21 runtime.
- `x86_64`: Intel Mac. Use an x64 Java 21 runtime.

Platform-specific JavaFX JARs may exist in developer builds, but are not part of
the universal release distribution.

Installing a JDK distribution that includes JavaFX can work around some JavaFX startup problems, but it is not a universal fix. The JavaFX runtime still needs to match the Mac architecture.

### The app cannot open a file

Check that the file is one of:

- `.pdf`
- `.xml` using CII or UBL syntax

For PDF files, the PDF must contain embedded Factur-X XML.

### The app will not close while it is processing

Choose `Exit and cancel` in the confirmation dialog. This stops the active operation and closes the app; any unsaved edits remain unsaved.

### Save fails

Saving can fail when:

- The edited XML is not well-formed or exceeds the input limit.
- The edited XML does not pass validation.
- The PDF cannot be rewritten safely or its Factur-X/PDF/A-3 container remains invalid.
- The file is open in another app.
- The folder is read-only.

For a signed Factur-X PDF, the normal save attempt always runs first. If edited XML has blocking business-rule findings, the app still asks whether to choose `Save anyway`. If either save path then fails specifically because the preserved signature invalidates the staged PDF, choose the offered unsigned test copy and a separate `<stem>-invalid.pdf` destination. This is a diagnostic file with no valid signature; sign the final production PDF again. Cancelling the prompt leaves the source unchanged. Unrelated PDF, container, syntax, and schema failures keep the normal `Save failed` behavior.

Fix the reported issue, close other apps that may be using the invoice, and try again. The save dialogs keep the complete available validator details in a scrollable text area so long rule messages are not cut off.

### Peppol lookup fails

Check your internet connection and whether a firewall, proxy, or DNS policy blocks Peppol discovery. A timeout or lookup failure does not mean the participant is unregistered; it means the app could not obtain a reliable result. Click `Refresh` to try again.

## 15. Safe Use Recommendation

Before editing a real customer or production invoice, make a copy of the file and test the workflow on the copy.
