# Peppol-utils 1.3.1 End-User Guide

## Table Of Contents

- [What's New In 1.3.1](#whats-new-in-131)

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
9. [Validate The Invoice](#9-validate-the-invoice)
10. [Visualize Edited XML](#10-visualize-edited-xml)
11. [Convert Between UBL And CII](#11-convert-between-ubl-and-cii)
12. [Generate France CTC Flow 1 XML](#12-generate-france-ctc-flow-1-xml)
13. [Save Changes](#13-save-changes)

**Help and safe use**

14. [Troubleshooting](#14-troubleshooting)
15. [Safe Use Recommendation](#15-safe-use-recommendation)

## What's New In 1.3.1

This guide applies to the `peppol-utils-1.3.1.jar` desktop app. Version 1.3.1 fixes problems found after 1.3.0. Validation rules and conversion are unchanged.

- `Visualize` and `XML to PDF` work even if you have not opened the `Visualize invoice` tab yet. An unexpected failure now says so in plain words.
- Converted or generated XML is marked `Unsaved` until you save it, and closing the app or opening another invoice asks before discarding it. The status bar shows the state of the tab you are looking at.
- The app starts on a `No invoice open` screen with an `Open invoice` button, and the status bar no longer says work is running after it has stopped or was cancelled.
- After validation the findings fill the space under the verdict; the invoice syntax, profile and VESID are in a section you can expand. In a narrow window the XML find bar moves its buttons to a second row instead of cutting them off.
- The France CTC Flow 1 dialog explains `Base`, `Full` and the BT-23 billing framework codes. `XML to PDF` is no longer coloured like an error.
- For UBL invoices the Summary's grand total is the total including tax, and the amount due stays the payable amount. Saving is safer: if a backup copy could not be completed, the original invoice is never replaced by it, and `Save as` uses the same backup-and-restore as saving in place.

### Changes in 1.3.0

Version 1.3.0 polished the workbench after 1.2.0. Invoice handling, validation rules and conversion are unchanged from 1.2.0.

- In a narrow window or at a large zoom the left sidebar shows only its icons; hover over one to see its command.
- Focus outlines appear only while you work with the keyboard, so clicking a button no longer leaves an outline on it.
- `Ctrl+Tab` switches tabs even from a field or value, and `Ctrl+F` opens the XML find bar from the tab strip or sidebar.
- A finished Peppol check or participant lookup sums up its Production and Test results in the status bar, and a screen reader reads it.
- Large invoices no longer freeze the window when they first open, and the Peppol results update without rebuilding the whole panel.

### Changes in 1.2.0

Version 1.2.0 made the workbench easier to read, operate from the keyboard and use with a screen reader. Invoice handling, validation rules and conversion are unchanged from 1.1.0.

- The workbench is easier to use from the keyboard and at larger text sizes (see [Keyboard shortcuts, interface zoom and cancelling](#keyboard-shortcuts-interface-zoom-and-cancelling)); validation results now say when they are out of date or could not run (with `Retry`), and findings can be filtered, copied and located in the XML (see [Validate The Invoice](#9-validate-the-invoice)).
- Control outlines, XML comments and line numbers are easier to see, the selected tab and validation row are marked by a bar as well as a tint, the interface follows Windows High Contrast, and a finished validation moves focus to the verdict (see the same section). Screen readers can read the XML editors and name every summary, validation and Peppol value by its label, and when Peppol results are rebuilt, keyboard focus returns to the Seller/Buyer box, `Check` button or lookup control you used (while a lookup runs and that control is disabled, focus waits on the lookup's progress indicator, named `Checking Peppol participant information` or `Looking up the Peppol participant`). The selected Seller/Buyer box is marked by a filled dot and a stronger edge, not only a tint.
- In a narrow window or at a large zoom, the `Validation` tab rearranges itself instead of cutting content off.
- Production/Test show a drawn icon for each result, so a failed lookup (error circle) is told apart from one not yet run (minus circle); hover over an environment to see its full result. The Peppol overview states each conclusion in plain words led by an icon instead of emoji.
- Toolbars wrap to a second row instead of cutting buttons off, and every icon grows with the interface zoom.
- With a screen reader running, the outcome of opening, saving, converting, formatting, visualizing or a Peppol check is read from the status bar (press `Esc` to return to where you were), and the XML search result is the Find field's help and a `Tab` stop of its own.

Version 1.1.0 upgraded the bundled validation and conversion libraries:

- Validation uses the latest rule versions bundled with the app: Factur-X 1.0.9-2, France CTC 1.4-04, and France PPF Flux 1 and Flux 10 1.1.
- With `France CTC` selected, Factur-X BASIC WL, EN 16931, and EXTENDED invoices are checked with the dedicated France CTC Factur-X rule sets.
- Generated Flow 1 output keeps only the VAT totals that Flux 1 rule G1.12 allows (one in EUR for EUR invoices; otherwise one in the invoice currency and one in EUR), and generated UBL Flow 1 output now passes schema validation.
- The Flow 1 Base trajectory is labelled `Démarrage` correctly.
- The AI Wiki (Knowledge Base) has been removed. The app no longer downloads documentation bundles or contacts Ollama, Gemini, or OpenAI; the only network access is an explicit Peppol lookup. Data left by earlier versions is not used and can be deleted manually: the AI Wiki settings file `~/.peppol-utils/knowledgebase.json` (it may contain saved Gemini/OpenAI API keys) and the downloaded bundles in `%LOCALAPPDATA%\Peppol-utils\knowledge-base` on Windows.

Version 1.0.0 internally decomposes the large `FacturxService` implementation into focused components while preserving the public `FacturxService` facade, reusable `com.facturxutils` API, invoice workflows, validation semantics, and save guarantees. It retains the low-resource improvements delivered through 0.9.0: bounded startup and working memory, viewport-sized PDF rendering, and short-lived workers for validation, conversion, France CTC Flow 1 generation, and XML-to-PDF visualization.

- Continue using the same supported invoice workflows and API-compatible integrations after the internal service decomposition.
- Choose `Save anyway` for a Factur-X PDF when edited XML is well-formed but has schema, profile, or business-rule findings; the rewritten PDF still has to pass Factur-X container and PDF/A-3 checks.
- Read complete validation and save-failure details in resizable, scrollable dialogs, including rule IDs, locations, artefacts, failed tests, and validator roles.
- Repair supported Factur-X XMP/output-intent and attachment metadata defects while re-embedding XML. The safe staging repair can remove stale Factur-X attachment references, remove an empty AcroForm with no widgets, and embed mapped Standard-14 Helvetica/Times-family font substitutions, including nested form resources.

- Start in a centered, right-sized window instead of allocating a maximized graphics surface immediately.
- Preview PDFs with a bounded viewport-aware renderer designed for integrated GPUs and low-memory computers.
- Load invoice validation and visualization only when their workflows need them.
- Edit large XML documents without applying expensive whole-document syntax highlighting above the safe interactive limit.

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
java -jar .\peppol-utils-1.3.1.jar
```

## 4. Open An Invoice

Until an invoice is open, `Summary` shows `No invoice open` with its own `Open invoice` button instead of empty invoice fields.

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

### Keyboard shortcuts, interface zoom and cancelling

| Shortcut | Action |
| --- | --- |
| `Ctrl+O` | Open an invoice |
| `Ctrl+S` | Save changes (when there are unsaved changes) |
| `F5` or `Ctrl+Enter` | Validate the current XML |
| `Ctrl+Tab` / `Ctrl+Shift+Tab` | Next / previous workbench tab (also from a field or value; focus moves to the tab header, as it does whenever the tab you were working in is left) |
| `Ctrl+F` | Search in the XML editor (the invoice's, or a generated or converted tab's), also with focus on the tab strip or sidebar while that tab is shown |
| `Ctrl+W` | Close the selected generated or converted XML tab (asks first when it has unsaved changes) |
| `Ctrl+=` / `Ctrl+-` / `Ctrl+0` | Make the whole interface larger / smaller / back to 100% (80% to 160%, remembered between sessions) |
| `Enter` / `Ctrl+C` in the findings table | Show the selected finding in the XML / copy it |
| `Esc` | Cancel the running operation |
| `Enter` / `Shift+Enter` in the Find field | Next / previous match (typing searches after a short pause) |

Buttons, lists, tabs and the Seller/Buyer boxes show a focus outline only while you work with the keyboard; clicking one with the mouse does not leave an outline on it. Text fields always show where you are typing.

The shortcuts are shown in the tooltips of `Open invoice`, `Save changes` and `Validate`, and a screen reader announces them as each command's help text. The row menu of the findings table lists its keys in the menu text (`Show in XML (Enter)`, `Copy finding (Ctrl+C)`); they only act in the table. They do nothing while the button they stand for is disabled. On macOS, `Ctrl` is `⌘`.

While validation, opening, conversion, Flow 1 generation or visualization is running, a `Cancel` button (or `Esc`) appears under the progress indicator. It stops that operation's background worker, leaves your document and the previous results untouched, and reports `... cancelled.` in the status bar. Saving and repairing cannot be cancelled from there, because they must finish or restore the original file. While the progress overlay is shown, `Tab` stays on the `Cancel` button rather than moving to the controls behind it.

While one of these operations runs, every XML editor (the invoice's and those of generated or converted documents) is read-only and `Ctrl+Tab` and `Ctrl+W` do nothing, so what is saved or validated is exactly what you see. `Format XML` also runs in the background now, so formatting a very large document no longer freezes the window, and editing a generated document stays responsive because its syntax colours refresh after a short pause.

When a `Validate` run finishes and you have not moved to another tab or control in the meantime, keyboard focus moves to the verdict at the top of the `Validation` tab (for example `Validation failed`), so a screen reader reads the outcome. If you have switched away, focus is left where you put it.

With a screen reader running, the same happens for the other operations: when opening, saving, converting, formatting or visualizing finishes, or a Peppol check you started finishes while focus is still in its panel, focus moves to the status message at the bottom of the window so its outcome is read, and `Esc` returns focus to the control you started from. Read-only values (the Summary and Peppol values, the validation verdict and the status message) are `Tab` stops only while a screen reader is running; without one, `Tab` moves between controls only, and you can still click a value to select and copy it. In the XML Find field, the result (`Find result: 2 / 5`, `Find result: No matches`) is the field's help text, which a screen reader reads when the field takes focus or when you ask for the item's details; it is not spoken by itself as you type, so you can also `Tab` past `Previous`, `Next` and `Close` to the result.

All pixel widths follow the interface zoom, including the conversion buttons, the Summary and Validation labels, the validation table columns and the About dialog. In a narrow window or at a large zoom the left sidebar shows only its icons, so the work area keeps its room; hover over an icon to see its command, and over the profile icon to see the current validation profile. The `Validation` tab also rearranges itself instead of cutting content off: the details of the selected finding move under the table, `Repair PDF` moves under the verdict, the filters above the table wrap to their own row, the `Validation set` column is hidden (the set is still shown in the details and the filter), and the tab scrolls when the window is too short. When Windows High Contrast is on, the workbench uses your high-contrast colours instead of its own palette; the selected row, tab and finding stay marked by an underline or bar rather than by colour alone.

Dialogs that could lose work (`Save changes` over the original file, `Discard`, `Save anyway`, `Save unsigned test copy`, `Exit and cancel`) never take `Enter` as their default: `Enter` or `Esc` always chooses `Cancel` / `Keep working`. Failures show a short cause and what to try next; click `Show Details` for the full technical message.

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

- `<` and `>` (`Previous page` / `Next page`) to move between pages.
- `-` and `+` (`Zoom out` / `Zoom in`) to change the page size.
- The scrollbars to move around the page when it is larger than the view.

The PDF page is centered in the view with padding around it. The PDF tab is only available when the opened invoice is a PDF.

## 7. Edit XML

Open the `XML` tab.

Available actions:

- Edit the XML directly in the editor.
- Click `Format XML` to pretty-print the XML.
- Click `Copy XML` to copy the whole XML to the clipboard.
- Use the `-` and `+` controls to adjust XML editor text size from 75% to 200%.
- Press `Ctrl+F` to search in the XML.
- Use `Previous`, `Next`, and `Close` in the search bar. In a narrow window or at a large interface zoom they move below the search field.
- Click `Visualize` to generate a PDF preview from the current XML.
- Click `Save as...` to write the current invoice to a new PDF or XML file without first choosing the overwrite/save-copy dialog.
- Use the editor scrollbars to move vertically or horizontally through long XML files.

When you edit the XML, the app marks the invoice as `Unsaved`.

A screen reader reads the editor as a text area named `Invoice XML`: it can read the XML line by line, follow the caret and announce selected text. `Show in XML` also states the line and column in the status bar.

Generated and converted XML tabs have the same editor (named after the tab, for example `Converted UBL XML`), the same text-size controls, `Copy XML` and `Ctrl+F` search, plus a `Close tab` button. A generated or converted document counts as `Unsaved` (with `*` after its tab name) until you save it with `Save as...`, and again after any later edit. `Close tab`, `Ctrl+W`, the tab's own close button, opening another invoice and exiting all ask before discarding it.

The `Saved` / `Unsaved` pill at the right of the status bar describes the selected document: a generated or converted tab's own state while that tab is selected, otherwise the opened invoice's.

## 8. Check Peppol Participant Information

The `Summary` tab shows a `Peppol participant information` area for the seller and buyer. The app reads only the standard electronic-address fields from the invoice and first checks locally whether each address has an eligible Peppol participant scheme. It does not substitute a tax ID, legal ID, generic party ID, or email address.

To check publication status:

1. Open the `Summary` tab.
2. Review the detected seller and buyer electronic addresses and schemes.
3. Click `Check Participant Information`.
4. Review the separate `Production` and `Test` results. When the check finishes, the status bar sums them up, for example `Peppol check finished. Seller: Production registered, Test registered. Buyer: Production not registered, Test registered.`
5. Review the invoice-type badge in each Production and Test row, then expand a result to inspect the discovery details. These can include the SML DNS zone, SMP URL, a selected Billing Invoice access point (active when available, otherwise the earliest future or another inactive endpoint), Document Type details, and optional Business Card company names, country, and registration date when the SMP provides them. Document Type details are collapsed by default; expand one to see its blue bold name, scheme and identifier/value, plus each matching process pair in its own card with service activation, expiry, and lifecycle status. The collapsed `SMP` header shows the SMP URL; expand it to inspect the signing certificate details embedded in validated SMP XML signatures (subject, issuer, serial, validity, key, and certificate signature algorithm) when available. This is distinct from both the SMP server TLS certificate and the AP endpoint certificate. The collapsed `Access Point` header shows the selected endpoint URL and lifecycle status; expand it under `SMP & AP` to inspect the transport, URL, service, contact, certificate, activation, expiry, and status metadata once for the lookup result when the SMP supplies it. A `Not yet active` endpoint is published metadata, not a currently usable route. The certificate fields describe the AP endpoint certificate published in SMP service metadata; the SMP server TLS certificate is not captured. Click `Refresh` to bypass the short-lived cache and run the checks again.

Production and Test are checked independently. The Peppol overview reports two separate conclusions: document-type publication (`Published by SMP`, `Not published by SMP`, or `Publication unknown`) and advertised process/endpoint metadata (`Active AS4 v2 endpoint metadata available`, `Active AS4 v2 endpoint metadata available; choose a process`, `No matching active endpoint`, or `Process and endpoint unresolved`). Each conclusion starts with an icon: a check when supported, a warning triangle when not, a clock while checking and an info circle when it needs your choice or could not be resolved. Document publication uses the exact Peppol document type identifier first (case-sensitive scheme and value), then the standard Peppol Wildcard Scheme. An exact same-scheme/value registration takes precedence; otherwise the selector chooses the wildcard with the greatest number of matching CustomizationID parts. A published document type is not by itself proof that a requested process can be delivered; an active metadata row is not an AS4 reachability test.

For a recognized Factur-X hybrid PDF with explicit France CTC buyer routing (`schemeID=0225`), the comparison uses the French `pdf+xml` Factur-X transport type, not the embedded CII XML type. The embedded XML remains the source for invoice data and validation. A recognized Factur-X PDF without that routing context remains unresolved; the embedded CII type is never used as a fallback. Standalone XML carries its raw DDD BT-23/ProfileID process into the comparison, and a value that is not an exact member of the matched DTI's allowed process set is rejected. A Factur-X PDF's embedded BusinessProcess value is not treated as its Peppol transport process, so only that PDF case leaves process selection unresolved and exposes the allowed active processes advertised for the matched PDF registration instead. Test support does not imply Production support.

![Peppol participant information showing separate Production and Test results, discovery details, Business Card data, Document Type details, and collapsed SMP signing-certificate and Access Point panels.](docs/images/peppol-participant-information.png)

*Peppol participant information in the Summary tab. Production and Test are reported independently; this example shows a participant registered in Test but not in Production, with the invoice type unsupported in both environments.*

Registration and invoice-type support are separate results. A participant can be registered while not publishing the exact document type required by the open invoice. Possible registration results are registered, not registered, and lookup failed. Each Production/Test option shows an icon: a green check when registered, an amber warning triangle when not registered, a grey error circle when the lookup failed, a grey minus circle when not checked yet and a clock while checking; hover over it to see the full result. The icons grow with the interface zoom. Editing the invoice keeps the Peppol results as long as the seller and buyer electronic addresses, document type and process stay the same. A missing address, missing or invalid `schemeID`, email address, or non-registrable scheme is explained without sending a lookup.

Peppol lookup requires internet access. It queries Peppol SML/SMP discovery services only when you explicitly run a lookup; opening and validating an invoice do not automatically contact the network. A Peppol result describes discovery metadata at the time of the check and is not a guarantee that a future document transmission will succeed.

### Look up any Peppol participant

Use `Utilities` > `Peppol lookup` in the left sidebar when the participant is not taken from the open invoice.

1. Enter an identifier such as `0225:152857363`. The full `iso6523-actorid-upis::0225:152857363` form is also accepted.
2. Click `Look up participant` or press Enter.
3. Review the separate Production and Test results below the search box. The status bar sums them up when the lookup finishes (`Peppol lookup of 0225:152857363 finished: Production registered, Test not registered.`).
4. Expand the environment details to inspect its SML/SMP source, the collapsed SMP panel with signing certificate details from validated XML signatures, Document Type details, the collapsed Access Point panel with the selected Billing Invoice endpoint URL, lifecycle status, and metadata, and optional Business Card. SMP signing certificates, AP endpoint certificates, and the SMP server TLS certificate are distinct; the TLS peer certificate is not captured.

The identifier is checked locally before any request is sent. Each lookup is user initiated, Production and Test stay independent, and a network failure remains distinct from a participant that is not registered.

## 9. Validate The Invoice

Choose the `Validation profile` in the left sidebar:

- `Default`: general Factur-X / EN 16931 validation.
- `France CTC`: Flux 1 and Flux 10 use the Helger phive rule sets automatically. Flux 1 CII/UBL selects the Base/Démarrage or Full/Cible VESID from BT-24/CustomizationID, while the namespace-less Flux 10 `<Report>` selects the latest bundled `fr.ctc.flux10:report` set (1.1). Other documents first validate the exact detected invoice schema/profile VESID, then run the legacy 1.3.1 and latest bundled France CTC rule sets for CII/UBL EN 16931, CII/UBL Extended, UBL CreditNote, Factur-X, or CDAR. Factur-X BASIC WL, EN 16931, and EXTENDED use the dedicated France CTC Factur-X rule sets. The legacy set is skipped if it is not bundled.

Click `Validate`.

For a saved Factur-X PDF, the default validation covers the complete hybrid document: Kaltblut checks the Factur-X/ZUGFeRD BR-HYBRID container and XMP rules, veraPDF checks PDF/A-3 conformance, and DDD/phive check the embedded XML schema and business rules. PDF/A findings include the veraPDF rule and PDF object location. Container and XMP checks also run when `France CTC` is selected, alongside that profile's XML validation route.

Before validation is run, the `Validation` tab says `Not validated yet`; opening an invoice does not imply that it passed. Below the verdict, the collapsed `Invoice syntax, profile and VESID` section repeats the detected XML syntax, profile, guideline, and VESID from Summary; expand it when you need them, so the findings keep the space under the verdict.

After validation, the headline gives the overall result: `Validation passed`, `Validation passed with warnings`, or `Validation failed`. It names the validation set when only one set is responsible, or the number of sets otherwise. The line under it reads `Profile: <profile> · Ran: <sets>` and lists each validation set that reported findings as passed, passed with warnings, or failed (for example `Invoice profile rules — failed (1 error)`). The validators do not report which rule sets executed, so a set that produced no findings at all is not listed; the line never claims a set ran when it reported nothing. Passed checks and profile-detection messages are not listed as rows.

The table shows only errors and warnings. Blocking errors come first, then warnings; within each group the current rule sets come before the legacy `FR CTC old` set. The first blocking issue is selected for you. Use the two lists above the table to show only errors or warnings, or only one validation set; the counter then reads `2 of 7 issues`.

Select an issue to inspect, in the details panel beside the table (under it in a narrow window), its rule ID, description, element XPath, and expected or allowed values when the validator references a bundled code list. Expand `Technical details` for the validator test, engine/layer, rule artefact, source document, and Schematron role when supplied. The technical validator test is the XPath/Schematron condition evaluated by the validation engine; it is diagnostic logic, not usually a value to paste into the invoice.

For the selected issue you can:

- Click `Show in XML`, press `Enter`, or double-click the row to move the XML editor to the element named by the finding's XPath. This is best effort: the element is matched by name and position among its siblings, namespace prefixes are ignored, and the status bar says so when the element cannot be found or the finding has no element location (for example a PDF container finding). If the results are out of date the element may have moved.
- Click `Copy finding` (or press `Ctrl+C` in the table) to copy that finding with its validation set, rule, locations, test and guidance.
- Use `Copy report` above the table, or the row's right-click menu, to copy every finding.

The table columns are:

- Severity
- Validation set
- Rule code
- Message

Two situations never look like a normal result:

- **Results out of date.** If you edit the XML or change the validation profile after a run, the headline turns neutral and reads `Results out of date — XML changed` (or `— validation profile changed`) and the table is dimmed. Nothing is re-evaluated until you click `Validate` again. Opening a CDAR message switches the profile to `France CTC` automatically and says so in the status bar.
- **Validation could not run.** If the validator itself fails (for example its worker process cannot start), the tab shows `Validation could not run` with the cause and a `Retry validation` button. The earlier report is cleared, so it cannot be mistaken for a verdict on the current XML.

If there are no errors or warnings, the table says so. Findings are grouped under user-facing validation set names: `Invoice profile rules` (the detected schema/profile VESID, run by phive), `FR CTC` (the latest bundled France CTC version), `FR CTC old` (the legacy 1.3.1 set), and `Factur-X PDF container`; engine names such as PHIVE and Kaltblut stay in the technical details. Both France CTC versions run when 1.3.1 is available in Helger phive; if a future dependency release removes it, only the latest set runs. Warnings remain non-blocking; validator execution failures and error/fatal assertions block strict saving.

Flux 1 trajectory selection uses `urn.cpro.gouv.fr:1p0:einvoicingextract#Base` for Démarrage and `...#Full` for Cible. The service reports the latest bundled `fr.ctc.flux1:*` VESID (currently 1.1) in the ordinary PHIVE validation set. Generated Flow 1 output still applies the field catalog and temporary operational mapping before running the same phive-backed completeness check.

![France CTC validation results showing an error and warning for the same rule in the current and legacy rule sets.](docs/images/france-ctc-validation-results.png)

*This screenshot comes from an earlier build and is not regenerated for every release: the product name, the sort order (blocking issues first) and the filter and action controls above the table differ from what you see now.*

*France CTC validation result. Select a row to see the rule ID, description, element/context, and technical details in the panel on the right. The same assertion can appear once for the current France CTC rules and once for the legacy `FR CTC old` rules.*

If the XML editor contains unsaved changes, validation checks the edited XML because those bytes are not yet embedded in the saved PDF. Save and reopen the PDF to validate the updated hybrid container and XMP metadata.

## 10. Visualize Edited XML

Open the `XML` tab and click `Visualize`.

The app generates a PDF preview from the current XML and opens the `Visualize invoice` tab.

Use:

- `<` and `>` (`Previous page` / `Next page`) to move between generated preview pages.
- `-` and `+` (`Zoom out` / `Zoom in`) to change the page size.
- `Save as...` to export the generated PDF visualization.
- The scrollbars to move around pages larger than the view.

This preview helps you inspect how the XML invoice content could look as a readable document.

## 11. Convert Between UBL And CII

Select `Conversion` in the left sidebar (or its tab; the sidebar button is highlighted while that tab is showing). The screen uses the current XML editor content, including unsaved edits.

- Click `UBL to CII` for a UBL 2.1 Invoice or CreditNote. The app opens a `Converted CII` tab.
- Click `CII to UBL` for CII / Factur-X XML. The app opens a `Converted UBL` tab.
- Click `XML to PDF` to open a generated preview in the `Visualize invoice` tab.
- Click `France CTC Flow 1` to generate a France CTC Flow 1 XML document from supported CII or UBL input.

Each syntax-conversion button is enabled only when the current XML has the matching input syntax. CII input can be:

- A standalone `.xml` file using CII syntax.
- A Factur-X PDF whose embedded invoice XML is CII.

UBL input is a standalone `.xml` UBL Invoice or CreditNote. The original invoice is not overwritten.

In the result tab, you can:

- Edit or format the generated XML, search it with `Ctrl+F`, or copy it with `Copy XML`.
- Click `Save as...` and choose where to write the CII or UBL XML file.
- Click `Close tab` (or press `Ctrl+W`) when you are done.

The suggested output names follow this pattern:

- UBL `invoice.xml` becomes `invoice.cii.xml`.
- UBL `invoice.ubl.xml` becomes `invoice.cii.xml`.
- CII `invoice.xml` becomes `invoice.ubl.xml`.
- CII `invoice.pdf` becomes `invoice.ubl.xml`.

In these names, `cii` or `ubl` labels the generated XML syntax; the file extension remains `.xml`.

For France CTC compatibility, the app also cleans up converted UBL identifiers that have no `schemeID` in contexts where France CTC requires one. The app does not invent a scheme. Properly schemed identifiers are kept.

## 12. Generate France CTC Flow 1 XML

Open any supported CII or UBL invoice, then select `Conversion` and click `France CTC Flow 1`.

1. Choose `Base` or `Full`. `Base` is selected by default. The dialog explains each: `Base` has the mandatory Flow 1 fields only, and `Full` the Base fields plus the additional Flow 1 fields.
2. For ordinary invoices, choose the applicable France CTC billing framework (BT-23) from the required list.
   The code's letter says what is billed (B goods, S services, M both) and its number the case
   (1 ordinary, 2 already paid, 4 final after a deposit, 5 subcontractor, 6 co-contractor, 7 already e-reported).
   The app does not guess a framework when the source does not contain one. For B8/S8/M8
   and B9/S9/M9 source invoices, the generated split documents keep their per-unit
   billing frameworks from the source invoice; no framework selection is required.
3. Click `Generate`.
4. Review the generated `France CTC Flow 1 Base (...)` or `France CTC Flow 1 Full (...)` tab.
5. Use `Format XML` or edit the XML if needed.
6. Click `Save as...` and choose an output location.

The generator keeps the source syntax: CII input produces CII F1 XML, while UBL invoice or credit-note input produces UBL F1 XML. It follows the XP Z12-012 v1.4 field subset and the bundled mapping resources. Ordinary invoices open one generated tab. B8/S8/M8 multi-seller invoices open one tab per unit-invoice number, and B9/S9/M9 bidirectional invoices open the two required directions. Generation computes validation findings through the matching dependency-supplied phive VESID selected from BT-24/CustomizationID. Generated tabs intentionally save the current XML without validation, including invalid or malformed XML needed for QC testing against a Peppol system. To inspect a saved output's findings, reopen it and use Validation. The app does not claim a separate bundled AIFE Schematron validation route.

The source invoice is not overwritten. The suggested filename is prefixed with the profile; split outputs also include the unit-invoice identifier.

## 13. Save Changes

Click `Save changes`.

The app shows the complete source path and lets you either overwrite the opened invoice or use `Save a copy...`. `Enter` in this dialog chooses `Cancel`, not the overwrite. Saving a copy stages the new file before replacing the selected destination, so the original invoice remains unchanged.

Important:

- The app does not create a backup copy.
- Make your own copy of the invoice before editing if you need to keep the original.
- For standalone XML files, the app validates the edited XML before overwriting the XML file.
- For PDF files, the app validates the edited XML, re-embeds it into the PDF, validates the rewritten PDF, and then replaces the original PDF.
- If validation finds blocking `ERROR` or `FATAL` findings, the app shows the complete rule IDs, messages, locations, and available validator details under `Show Details` in a dialog and asks whether you want to save the invalid XML anyway. The dialog opens with the error and warning counts for each validation set and offers `Review in Validation tab`, `Save anyway` and `Cancel`; the app does not switch tabs until you answer, and `Cancel` is the `Enter` default. For a Factur-X PDF, `Save anyway` permits XML schema, profile, or business-rule findings, but the embedded XML must still be readable and the rewritten PDF must pass Factur-X BR-HYBRID/XMP, attachment, and PDF/A-3 checks. For a standalone XML file, the explicit low-level save option retains its historical ability to write invalid XML. Choose this only when you intentionally need an XML-invalid file for testing.
- PDF re-embedding uses Mustangproject's `ZUGFeRDExporterFromA3`. When the source has damaged or missing hybrid metadata such as XMP or output intents, the save path may rebuild that container metadata from a sanitized PDFBox staging copy. That staging repair removes only Factur-X attachment references, preserves unrelated name trees and associated files, can remove an empty AcroForm only when it has no fields or widget annotations, and can recursively replace unembedded Standard-14 Helvetica/Times-family fonts with embedded TrueType substitutions. Supported substitutions retain widths and any existing ToUnicode map where possible, but may normalize a non-PDF/A source encoding to WinAnsi; the final Kaltblut and veraPDF checks remain mandatory, including for PDF/A-3U Unicode requirements. It does not flatten live forms, remove widgets or signature fields, or replace arbitrary/unmappable fonts. A save error opens a short cause and next step, with the full available details under `Show Details`.
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
java -jar .\peppol-utils-1.3.1.jar
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

Fix the reported issue, close other apps that may be using the invoice, and try again. The invalid-save dialog keeps the complete available validator details under `Show Details` so long rule messages are not cut off, and error dialogs keep the raw technical message there.

### Peppol lookup fails

Check your internet connection and whether a firewall, proxy, or DNS policy blocks Peppol discovery. A timeout or lookup failure does not mean the participant is unregistered; it means the app could not obtain a reliable result. Click `Refresh` to try again.

## 15. Safe Use Recommendation

Before editing a real customer or production invoice, make a copy of the file and test the workflow on the copy.
