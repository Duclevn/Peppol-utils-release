# Peppol-utils 1.8.2 User Guide

Peppol-utils opens an electronic invoice on your computer. You can read it, edit its XML, validate it, convert it between UBL and CII, save it, and look up the buyer's or seller's Peppol registration.

It opens a **Factur-X PDF** (a PDF that carries the invoice XML) or a standalone **`.xml`** invoice in CII or UBL syntax.

## Install

Download the files from the [latest release](https://github.com/Duclevn/Peppol-utils-release/releases/latest).

- **Windows:** run `Peppol-utils-<version>-windows-x64-setup.exe`, then start Peppol-utils from the Start menu. No Java is needed.
- **Any system:** use `peppol-utils-<version>.jar` with Java 25 or newer: `java -jar peppol-utils-<version>.jar`. It runs on Windows x64, Linux x64/ARM64 and macOS Intel/Apple Silicon.

## Open an invoice

Click `Open invoice`, choose `File` > `Open...`, or press `Ctrl+O`. In Windows you can also right-click a `.pdf` or `.xml` file and choose `Open with` > `Peppol-utils`.

Opening an invoice never validates it and never uses the network.

## The Invoice tab

![The Invoice tab with the Overview card and the invoice rendered from its XML.](docs/images/invoice-tab.png)

The `Invoice` tab has two cards:

- `Overview`: invoice number, type, date, amounts, seller, buyer, and the detected format, profile and validation rules (VESID).
- `Document`: for a Factur-X PDF, a switch between `Original PDF` and `Visualize from XML`. A standalone XML is always shown rendered from its XML. Use `⋯` for `Render again` and `Save PDF as…`.

## The XML tab

The `XML` tab holds the invoice XML. Edit it directly, use `Format XML`, search with `Ctrl+F`, or `Copy XML`. Edits stay unsaved until you save (see below), and `Visualize` shows how your edits render.

## Validate

Click `Validate`, or press `F5`. Its arrow picks the rule set:

- `Auto`: the rules for the detected syntax and profile.
- `France CTC`: also checks the France CTC rule sets, and Flux 1 and Flux 10 documents.

For a Factur-X PDF, validation also checks the PDF container and PDF/A-3.

The result appears in the validation line at the bottom: `Passed`, `Passed with warnings` or `Failed`, with the rule set and the time. Click it to open the drawer.

![The validation drawer listing findings, with the selected finding's details at the right.](docs/images/validation-drawer.png)

- Each finding shows its rule ID, validation set, XML line and message. The chips `All`, errors and warnings filter the list.
- Select a finding to read its details, then click `Show in XML` to jump to its element. `Technical details` holds the validator's own data.
- `Copy report` and `Copy finding` put the text on the clipboard.
- After you edit the XML, the result is marked `Out of date` until you validate again.

### A missing element

When a finding is about a business term that is not in the XML (for example `BT-27 Seller name`), the XML tab shows a **ghost line** where that element belongs: a light red line, outside the text, with the expected start tag and `Missing: BT-27 Seller name · BR-06`. `Show in XML` puts the cursor there. Nothing is added to your XML.

![A ghost line for the missing seller name (BT-27), with the finding in the drawer.](docs/images/missing-element-ghost-line.png)

## Convert

The `Convert` menu works on the current XML, including unsaved edits. Each result opens in its own tab; your invoice is not changed.

- `UBL to CII` and `CII to UBL` change the syntax.
- `France CTC Flow 1` generates Flux 1 XML in `Base` or `Full` form. For an ordinary invoice, choose its billing framework (BT-23); the app does not guess it.

An item that cannot run for the open XML is greyed out and says why. In a result tab, use `Format XML`, `Save as...` or `Close tab`.

![A UBL invoice converted to CII and to France CTC Flow 1 Full, each in its own tab.](docs/images/convert-flow1.png)

## Save

Click `Save changes`, or press `Ctrl+S`. The app asks `Replace the original invoice?`; choose to replace it or `Save a copy…`. It makes no backup, so keep your own copy of important invoices.

Saving is strict:

- The XML is validated first with the `Auto` rules. For a PDF, the XML is put back into the PDF and the rewritten PDF is checked again before it replaces the original.
- If the XML has errors, the app lists them and offers `Review findings`, `Save anyway` or `Cancel`. `Save anyway` is possible only when the XML is well-formed and the PDF container stays valid.
- If the file cannot be saved safely, the original is left as it was.

If only the PDF's Factur-X container is invalid, open the drawer and choose `⋯` > `Repair PDF container…`. It writes a separate `-repaired.pdf`.

## Peppol lookup

Check whether a participant is registered on the Peppol network.

- Click `Peppol lookup` beside the Seller or Buyer in the `Overview` card, or type a participant ID such as `0225:123456789` in the top bar's lookup field (`Ctrl+L`).
- The panel checks Production and Test separately and shows the invoice document types the participant publishes and their endpoints, plus the Business Card when there is one.

![The Peppol lookup panel showing a participant registered in Test, with its document types.](docs/images/peppol-lookup.png)

A result says what was published, not that delivery works. A lookup that fails (no internet, a firewall) is not the same as an unregistered participant: try again. Every lookup queries the network again; nothing is cached.

## Privacy

Invoices are processed on your computer only. The app uses the network only when you start a Peppol lookup, and nothing from the open invoice is sent. The app has no other online feature.

## Shortcuts

| Keys | Action |
| --- | --- |
| `Ctrl+O` / `Ctrl+S` | Open / save |
| `F5` or `Ctrl+Enter` | Validate |
| `Ctrl+F` | Search the XML |
| `Ctrl+L` | Peppol lookup field |
| `Ctrl+=` / `Ctrl+-` / `Ctrl+0` | Larger / smaller / normal interface |
| `Esc` | Cancel the running operation |

## Troubleshooting

- **The jar does not start:** run `java -version`; Peppol-utils needs Java 25 or newer.
- **"The native component of Peppol-utils could not start":** a security policy blocks `Peppol-utils.exe`. Ask your administrator to allow it, reinstall, or use the jar.
- **Save fails:** read the message. Common causes are invalid XML, a file open in another program, or a read-only folder.

## Report a problem

Open an issue at [github.com/Duclevn/Peppol-utils-release/issues](https://github.com/Duclevn/Peppol-utils-release/issues) with your version (`File` > `About Peppol-utils`), what you did and what you saw. Do not attach real customer invoices; use an anonymized copy.
