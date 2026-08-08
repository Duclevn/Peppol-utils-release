# Peppol-utils 0.7.0 End-User Guide

## Table Of Contents

- [What's New In 0.7.0](#whats-new-in-070)

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

## What's New In 0.7.0

This guide applies to the `peppol-utils-0.7.0.jar` desktop app. Knowledge Base retrieval uses local BM25 and reviewed multilingual terminology, selects authoritative passages, and limits provider context to focused evidence. Existing invoice workflows and the reusable core API remain compatible.

- Search a signed, versioned e-invoicing documentation bundle locally from `Utilities` > `AI Wiki`.
- Ask questions through local Ollama or optional Gemini and OpenAI providers, and inspect application-generated source cards.
- Continue using local source search when the selected provider is unavailable; questions with insufficient evidence are not sent to any provider.
- Convert UBL Invoice/CreditNote to CII and CII/Factur-X XML to UBL from `Conversion`.
- Generate Base or Full France CTC Flow 1 XML while retaining CII or UBL syntax.
- Validate Flux 1 and Flux 10 with the Helger phive rules selected from the Flux 1 BT-24/CustomizationID trajectory or the Flux 10 `<Report>` root.
- Check Peppol participant publication and exact invoice-type support separately in Production and Test.
- Use the dedicated two-layer France CTC validation route for Flux 2/CDAR documents.
- Review, edit, and save converted or generated XML in separate tabs without overwriting the source invoice.

## 1. What You Receive

The universal JAR contains the app plus the libraries it needs for PDF rendering, Factur-X extraction, XML editing, validation rules, UBL/CII conversion in both directions, and JavaFX controls. It supports Windows x64, Linux x64/ARM64, and macOS Intel/Apple Silicon. A self-contained Windows installer may also be supplied; it includes its own Java runtime.

Users do not need Maven or the source code.

## 2. Install Java

When using the JAR, Peppol-utils requires Java 21. The optional Windows installer already includes the required runtime.

Install a Java 21 runtime or JDK from a vendor you trust. JavaFX is bundled with the application; the universal JAR selects the matching native files for its supported operating system and CPU architecture.

Recommended download page:
- Oracle Java downloads: https://www.oracle.com/java/technologies/downloads/

## 3. Start The App

Put the jar in a folder you can find, for example `Downloads` or `Documents`.
Open PowerShell in that folder and run:
```powershell
java -jar .\peppol-utils-0.7.0.jar
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
5. Review the invoice-type badge in each Production and Test row, then expand a result to inspect the discovery details. These can include the SML DNS zone, SMP URL, a single active Billing Invoice access point, Document Type details, and optional Business Card company names, country, and registration date when the SMP provides them. Document Type details are collapsed by default; expand one to see its blue bold name, scheme and identifier/value, plus each matching process row. The collapsed `SMP` header shows the SMP URL; expand it to inspect the signing certificate details embedded in validated SMP XML signatures (subject, issuer, serial, validity, key, and certificate signature algorithm) when available. This is distinct from both the SMP server TLS certificate and the AP endpoint certificate. The collapsed `Access Point` header shows the selected endpoint URL; expand it under `SMP & AP` to inspect the transport, URL, service, contact, certificate, activation, expiry, and status metadata once for the lookup result when the SMP supplies it. The certificate fields describe the AP endpoint certificate published in SMP service metadata; the SMP server TLS certificate is not captured. Click `Refresh` to bypass the short-lived cache and run the checks again.

Production and Test have independent badges: `✔ Invoice type supported`, `⚠ Invoice type not supported`, or `❔ Invoice type unknown`. The comparison uses the invoice's exact Peppol document type identifier against the document types published in that environment. For a Factur-X EN 16931 PDF, it compares the French `pdf+xml` Factur-X transport type, not the embedded CII XML type. For standalone XML, it compares the XML syntax, Invoice/CreditNote type, customization/profile, and syntax version. Test support does not imply Production support.

![Peppol participant information showing separate Production and Test results, discovery details, Business Card data, Document Type details, and collapsed SMP signing-certificate and Access Point panels.](docs/images/peppol-participant-information.png)

*Peppol participant information in the Summary tab. Production and Test are reported independently; this example shows a participant registered in Test but not in Production, with the invoice type unsupported in both environments.*

Registration and invoice-type support are separate results. A participant can be registered while not publishing the exact document type required by the open invoice. Possible registration results are registered, not registered, and lookup failed. A missing address, missing or invalid `schemeID`, email address, or non-registrable scheme is explained without sending a lookup.

Peppol lookup requires internet access. It queries Peppol SML/SMP discovery services only when you explicitly run a lookup; opening and validating an invoice do not automatically contact the network. Knowledge Base has a separate, explicit network flow described below. A Peppol result describes discovery metadata at the time of the check and is not a guarantee that a future document transmission will succeed.

### Look up any Peppol participant

Use `Utilities` > `Peppol lookup` in the left sidebar when the participant is not taken from the open invoice.

1. Enter an identifier such as `0225:152857363`. The full `iso6523-actorid-upis::0225:152857363` form is also accepted.
2. Click `Look up participant` or press Enter.
3. Review the separate Production and Test results below the search box.
4. Expand the environment details to inspect its SML/SMP source, the collapsed SMP panel with signing certificate details from validated XML signatures, Document Type details, the collapsed Access Point panel with the selected Billing Invoice endpoint URL and metadata, and optional Business Card. SMP signing certificates, AP endpoint certificates, and the SMP server TLS certificate are distinct; the TLS peer certificate is not captured.

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

You can search locally even when the selected provider is unavailable. Questions in English, French, or Vietnamese use the signed bundle's BM25 index and a curated e-invoicing terminology catalog. When a configured provider is available, it may prepare bounded English, French, and keyword search variants before local retrieval. Follow-up retrieval uses the preceding user question to resolve the referenced standard or concept while the current question remains the one answered. For a question that asks for a list, count, or document-wide explanation, the app chooses the most relevant current source and supplies contiguous source pages rather than disconnected snippets. If no result meets the relevance threshold, the app reports insufficient evidence and does not request an answer from the provider.

For a synthesized answer:

1. For private local generation, start Ollama and install a chat model. For cloud generation, obtain your own Gemini or OpenAI API key.
2. Open `Settings` and choose a provider. Ollama connects only to `http://127.0.0.1:11434/api/`; cloud providers use their official HTTPS APIs.
3. For Ollama, select an installed model. For a cloud provider, enter the exact model name, then save the settings and API key or provide the corresponding environment variable. Each cloud provider remembers its own model; `Use default` restores the bundled default name.
4. Optionally click `Test connection`; the settings dialog confirms the connection or shows a visible error.
5. Type a question and press Enter or click `Send`. Press Shift+Enter to insert a new line. An animated `Assistant is thinking...` indicator remains visible until the first response text arrives.
6. Read the formatted Markdown answer, then click its source action to open the contextual source rail on the right. Sources explicitly cited as `[S1]`, `[S2]`, and so on are grouped by document with consolidated page and marker lists; repository paths and exact commits remain under `Source details`. If the provider returned no valid citation marker, the fallback source action is labeled `Retrieved sources supplied to Assistant`; local-search-only results use `Retrieved sources`.

Hover over a message to reveal its small copy icon inside the upper-right corner. `Chats` opens a session-history dialog; `New chat` keeps the current conversation available while starting a clean one. This lightweight chat history lasts for the current app session and is not written to disk.

The selected provider and cloud model names are remembered. Cloud API keys may be supplied through environment variables or saved in `~/.peppol-utils/knowledgebase.json`; the file is written atomically with owner-only POSIX permissions where supported, but its contents are not encrypted. Prefer environment variables or an operating-system credential workflow on managed devices.

The provider receives the question, at most six recent turns, and selected source excerpts. The app does not attach an invoice or send invoice XML/PDF content. `Stop` cancels an active response, and normal invoice features remain available if GitHub or the provider is offline.

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
- If validation finds fatal errors, the app shows those errors and asks whether you want to save the invalid XML anyway. Choose this only when you intentionally need an invalid file for testing.

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
java -jar .\peppol-utils-0.7.0.jar
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

Then use the matching app jar and Java build:

- `arm64`: Apple Silicon / M-series Mac, including M1, M2, M3, M4, and M5. Use the macOS Apple Silicon / AArch64 jar and JDK.
- `x86_64`: Intel Mac. Use the macOS Intel / x64 jar and JDK.

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

- The edited XML is not well-formed.
- The edited XML does not pass validation.
- The PDF cannot be rewritten safely.
- The file is open in another app.
- The folder is read-only.

Fix the reported issue, close other apps that may be using the invoice, and try again.

### Peppol lookup fails

Check your internet connection and whether a firewall, proxy, or DNS policy blocks Peppol discovery. A timeout or lookup failure does not mean the participant is unregistered; it means the app could not obtain a reliable result. Click `Refresh` to try again.

## 15. Safe Use Recommendation

Before editing a real customer or production invoice, make a copy of the file and test the workflow on the copy.
