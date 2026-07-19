# ImagePDF.tools

### Browser-native image and PDF utilities. Sharp, private, instant.

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/your-username/imagepdf/pulls)
[![Built with Next.js](https://img.shields.io/badge/Built_with-Next.js-black.svg)](https://nextjs.org)
[![Privacy: Browser-Native](https://img.shields.io/badge/Privacy-Browser_Native-blue.svg)](#privacy)

<hr />

ImagePDF.tools is a collection of browser-native tools designed for quick, private, and frictionless file operations. By executing all processing directly on your device via WebAssembly, canvas APIs, and client-side JavaScript, the platform guarantees that your files never leave your browser.

## Why We Built ImagePDF.tools

We built ImagePDF.tools to address the frustration of using modern file utilities. Most tools are built around marketing funnels rather than user workflows. They force sign-ups, impose arbitrary upload limits, and compromise privacy by sending documents to remote servers.

## The Problems We Saw

Most existing image and PDF tools suffer from persistent issues:

* **Server dependency**: Uploading sensitive legal documents, contracts, or private photos to external servers raises privacy risks.
* **Paywalls and limits**: Free tiers are often restricted by file size, batch size, or conversion counts.
* **Dark patterns**: Websites clutter interfaces with intrusive ads, force account creation, or insert unwanted watermarks.
* **Performance lag**: Uploading large files to a remote server, waiting for server-side queues, and saving the results is slow.

## Our Solution

ImagePDF.tools offers a sharp, private, and instant alternative:

* **On-device execution**: All processing runs locally in your browser using WebAssembly and canvas APIs. Files are never uploaded.
* **Account-free utilities**: Complete your task immediately without entering an email address or creating an account.
* **Unrestricted usage**: Perform file conversions and compressions without arbitrary limits.
* **Clean interface**: No ad clutter, no popups, and no visual noise. The tool itself is the interface.
* **Modern architecture**: Leverages Next.js, Tailwind CSS, and WebAssembly to maximize speed and responsiveness.

## Key Features

Why the architecture of ImagePDF.tools matters to users:

* **Absolute privacy**: Processing documents locally means zero risk of data leaks or server-side retention.
* **Instant processing**: Local CPU processing removes upload and download network bottlenecks.
* **Seamless handoffs**: Uses a custom Zustand store (`handoffStore`) to pass a processed file from one tool's output directly into another tool's input, eliminating the need to save and re-upload intermediate files.
* **Local edit history**: A custom Zustand store (`historyStore`) maintains a local history stack of up to 5 document snapshots, allowing users to undo adjustments or revert to a previous file state.
* **Flexible import options**: The dropzone accepts local files, supports clipboard pasting (using browser clipboard APIs), and integrates with Dropbox, Google Drive, and Microsoft OneDrive pickers.
* **Cross-platform PDF previewing**: Uses `pdfjs-dist` to render PDF pages onto client-side canvases, providing visual document previews across mobile and desktop browsers without relying on native iframe viewers.
* **Compare slider**: Features an interactive split-screen view (`CompareView`) with mouse-wheel zoom (up to 10x), panning, and pinch-to-zoom touch support to evaluate image compression quality.

## Under the Hood (Technical Architecture)

ImagePDF.tools performs complex compilation and image processing directly in the client browser.

### DOCX to PDF Compilation Pipeline
For Word to PDF conversion, the app bypasses server compilers entirely:
1. **Layout parsing**: Reads the ZIP container of the `.docx` using `jszip` to extract margins and identify document font families from `word/fontTable.xml`.
2. **Local Font Access API**: Queries `window.queryLocalFonts` to request permission to use matching, pre-installed computer fonts.
3. **Pandoc WASM**: Converts the document structure into Typst formatting.
4. **Typst VFS mapping**: Mounts the system fonts and extracted document media files into the Typst virtual filesystem.
5. **Typst WASM compilation**: Uses `@myriaddreamin/typst-all-in-one.ts` to compile the final PDF.

### Image Compression Router
Image compression uses format-specific algorithms to optimize outputs:
* **JPEG & WebP**: Uses compiled WebAssembly modules of MozJPEG and libwebp (via `@jsquash`) to achieve files 10% to 20% smaller than standard browser canvas serialization.
* **PNG**: Implements UPNG.js K-d tree palette quantization.
* **SVG**: Optimizes code structure via coordinate rounding and tag cleanups rather than rasterizing.
* **Fallback guarantee**: Automatically compares sizes and falls back to the original file if the compressed blob size exceeds it.

### High-Accuracy Client-Side OCR
OCR tools deliver flat memory consumption and clean outputs:
1. **Luminance pre-processing**: Grayscales canvas pixels using weighted luminance (`0.299 * R + 0.587 * G + 0.114 * B`) and binarizes at a 128 threshold. This clean black-on-white conversion increases Tesseract OCR accuracy by 5% to 10%.
2. **Sequential rendering**: Renders PDF pages sequentially to keep browser RAM usage flat (~30 MB to 50 MB per page).
3. **Dynamic DPI selection**: Uses 300 DPI on desktop for optimal character detection, and automatically scales to 150 DPI on mobile to prevent RAM crashes.
4. **Searchable compilation**: Uses `pdf-lib` to overlay an invisible layer of text boxes matching the bounding boxes parsed by `tesseract.js` workers.

### EXIF and Metadata Sanitization
* **JPEG**: Utilizes `piexifjs` to strip EXIF and GPS tags directly from the binary buffer with zero re-encoding and zero quality degradation.
* **PNG & WebP**: Rebuilds images from raw canvas RGBA bytes, naturally omitting original metadata.

## Available Tools

All tools run client-side on your device.

<table>
  <thead>
    <tr>
      <th align="left">Category</th>
      <th align="left">Available Tools</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>PDF Editing</strong></td>
      <td>Merge PDF, Split PDF, Rotate PDF, Number PDF, Organize PDF, Edit PDF</td>
    </tr>
    <tr>
      <td><strong>Image Editing</strong></td>
      <td>Resize Image, Crop Image, Flip Image, Rotate Image, Reduce Image Size</td>
    </tr>
    <tr>
      <td><strong>Conversion</strong></td>
      <td>Image to PDF, PDF to Image, JPG to PDF, PDF to JPG, PNG to WebP, WebP to PNG, JPG to WebP, WebP to JPG, JPG to PNG, PNG to JPEG, Word to PDF, PDF to Word</td>
    </tr>
    <tr>
      <td><strong>Compression</strong></td>
      <td>Compress PDF, Compress Image, Compress JPEG, Compress PNG</td>
    </tr>
    <tr>
      <td><strong>OCR &amp; Text</strong></td>
      <td>Image to Text (OCR), OCR PDF</td>
    </tr>
    <tr>
      <td><strong>Security &amp; Meta</strong></td>
      <td>Protect PDF, Unlock PDF, Watermark PDF, Metadata Editor, Remove Metadata</td>
    </tr>
  </tbody>
</table>

## Privacy

Privacy is not a feature promise: it is the structural architecture of the application.

* **Zero upload**: Files are loaded directly into the browser and processed in-memory. They are never sent to our servers.
* **Optional account features**: User accounts (managed via Clerk) and billing (managed via Stripe) are only used to authenticate users and handle Pro plan status. These systems never touch your documents or images.
* **No data footprint**: Once you close the browser tab, all session data and processed files are cleared from your device memory.

## Tech Stack

The application is built entirely on modern open-source technologies:

* **Framework**: Next.js 16 (App Router)
* **UI Library**: React 19
* **Styling**: Tailwind CSS 4
* **Language**: TypeScript
* **State Management**: Zustand
* **Postinstall copying**: Moves WASM packages (`pandoc.wasm`) and worker files (`pdf.worker.min.mjs`) into the public folder to enable client-side worker access.

## Project Goals

* Become the most intuitive, privacy-respecting PDF and image utility on the web.
* Maintain a fast, lightweight, and responsive workspace.
* Expand the collection of tools based on community requests.
* Retain a simple, focused interface that gets out of the user's way.

## Roadmap

* [ ] Add support for batch processing across all image conversion tools
* [ ] Implement smart PDF compression presets (high, medium, low)
* [ ] Introduce offline capabilities using progressive web app features
* [ ] Enhance OCR layout detection for complex document structures
* [ ] Add dark mode theme options across the workspace

## Contributing

Contributions are welcome to help improve the utilities:

1. Fork the repository.
2. Clone the fork to your local machine.
3. Install dependencies:
   ```bash
   npm install
   ```
4. Start the development server:
   ```bash
   npm run dev
   ```
5. Create a branch, make your changes, and open a pull request.

## License

This project is licensed under the MIT License. See the LICENSE file for details.

## Support

For questions, bug reports, or tool requests, please visit the support page or open a GitHub issue.

## Star the Repository

If you find ImagePDF.tools helpful, please star this repository. Your support helps others discover the project and encourages further development.
