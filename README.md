# ImagePDF.tools

### Free client-side PDF utilities and image tools. Private, offline-first, WebAssembly-powered.

<p align="center">
  <a href="https://imagepdf.tools"><strong>Visit ImagePDF.tools</strong></a>
</p>

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](https://imagepdf.tools)
[![Privacy: Browser-Native](https://img.shields.io/badge/Privacy-Browser_Native-blue.svg)](https://imagepdf.tools/privacy)
[![Built with Next.js](https://img.shields.io/badge/Built_with-Next.js-black.svg)](https://imagepdf.tools)

<hr />

[ImagePDF.tools](https://imagepdf.tools) is a collection of browser-native document utilities designed for quick, private, and frictionless file operations. It serves as a serverless, privacy-focused alternative to Smallpdf, iLovePDF, and TinyPNG. By executing all processing directly on your device via WebAssembly, canvas APIs, and client-side JavaScript, the platform guarantees that your files never leave your browser.

## The Problems We Solve

Most existing image and PDF tools suffer from persistent issues:

* **Server dependency**: Uploading sensitive legal documents, contracts, or private photos to external servers raises privacy risks.
* **Paywalls and limits**: Free tiers are often restricted by file size, batch size, or conversion counts.
* **Dark patterns**: Websites clutter interfaces with intrusive ads, force account creation, or insert unwanted watermarks.
* **Performance lag**: Uploading large files to a remote server, waiting for server-side queues, and saving the results is slow.

## Our Solution

[ImagePDF.tools](https://imagepdf.tools) offers a sharp, private, and instant alternative:

* **On-device execution**: All processing runs locally in your browser using WebAssembly and canvas APIs. Files are never uploaded.
* **Account-free utilities**: Complete your task immediately without entering an email address or creating an account.
* **Unrestricted usage**: Perform file conversions and compressions without arbitrary limits.
* **Clean interface**: No ad clutter, no popups, and no visual noise. The tool itself is the interface.
* **Modern architecture**: Leverages Next.js, Tailwind CSS, and WebAssembly to maximize speed and responsiveness.

## Tool Index & Backlink Directory

Click any tool link below to access the utility directly on [ImagePDF.tools](https://imagepdf.tools):

<table>
  <thead>
    <tr>
      <th align="left">Category</th>
      <th align="left">Available Tools (Direct Links)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>PDF Editing</strong></td>
      <td>
        <a href="https://imagepdf.tools/merge-pdf">Merge PDF</a>, 
        <a href="https://imagepdf.tools/split-pdf">Split PDF</a>, 
        <a href="https://imagepdf.tools/rotate-pdf">Rotate PDF</a>, 
        <a href="https://imagepdf.tools/number-pdf">Number PDF</a>, 
        <a href="https://imagepdf.tools/organize-pdf">Organize PDF</a>, 
        <a href="https://imagepdf.tools/edit-pdf">Edit PDF</a>
      </td>
    </tr>
    <tr>
      <td><strong>Image Editing</strong></td>
      <td>
        <a href="https://imagepdf.tools/resize-image">Resize Image</a>, 
        <a href="https://imagepdf.tools/crop-image">Crop Image</a>, 
        <a href="https://imagepdf.tools/flip-image">Flip / Rotate Image</a>, 
        <a href="https://imagepdf.tools/rotate-image">Rotate Image</a>, 
        <a href="https://imagepdf.tools/reduce-image-size">Reduce Image Size</a>
      </td>
    </tr>
    <tr>
      <td><strong>Conversion</strong></td>
      <td>
        <a href="https://imagepdf.tools/image-to-pdf">Image to PDF</a>, 
        <a href="https://imagepdf.tools/pdf-to-image">PDF to Image</a>, 
        <a href="https://imagepdf.tools/jpg-to-pdf">JPG to PDF</a>, 
        <a href="https://imagepdf.tools/pdf-to-jpg">PDF to JPG</a>, 
        <a href="https://imagepdf.tools/pdf-to-word">PDF to Word</a>, 
        <a href="https://imagepdf.tools/word-to-pdf">Word to PDF</a>, 
        <a href="https://imagepdf.tools/png-to-webp">PNG to WebP</a>, 
        <a href="https://imagepdf.tools/webp-to-png">WebP to PNG</a>, 
        <a href="https://imagepdf.tools/jpg-to-webp">JPG to WebP</a>, 
        <a href="https://imagepdf.tools/webp-to-jpg">WebP to JPG</a>, 
        <a href="https://imagepdf.tools/jpg-to-png">JPG to PNG</a>, 
        <a href="https://imagepdf.tools/convert-png-to-jpeg">PNG to JPEG</a>
      </td>
    </tr>
    <tr>
      <td><strong>Compression</strong></td>
      <td>
        <a href="https://imagepdf.tools/compress-pdf">Compress PDF</a>, 
        <a href="https://imagepdf.tools/compress-image">Compress Image</a>, 
        <a href="https://imagepdf.tools/compress-jpeg-online">Compress JPEG</a>, 
        <a href="https://imagepdf.tools/compress-png-online">Compress PNG</a>
      </td>
    </tr>
    <tr>
      <td><strong>OCR &amp; Text</strong></td>
      <td>
        <a href="https://imagepdf.tools/image-to-text">Image to Text (OCR)</a>, 
        <a href="https://imagepdf.tools/ocr-pdf">OCR PDF</a>
      </td>
    </tr>
    <tr>
      <td><strong>Security &amp; Meta</strong></td>
      <td>
        <a href="https://imagepdf.tools/protect-pdf">Protect PDF</a>, 
        <a href="https://imagepdf.tools/unlock-pdf">Unlock PDF</a>, 
        <a href="https://imagepdf.tools/watermark-pdf">Watermark PDF</a>, 
        <a href="https://imagepdf.tools/metadata-editor">Metadata Editor</a>, 
        <a href="https://imagepdf.tools/remove-metadata">Remove Metadata</a>
      </td>
    </tr>
  </tbody>
</table>

## Privacy Architecture

Privacy is not a feature promise: it is the structural architecture of [ImagePDF.tools](https://imagepdf.tools).

* **Zero upload**: Files are loaded directly into the browser and processed in-memory. They are never sent to external servers.
* **Optional account features**: User accounts (managed via Clerk) and billing (managed via Stripe) are only used to authenticate users and handle Pro plan status. These systems never touch your documents or images.
* **No data footprint**: Once you close the browser tab, all session data and processed files are cleared from your device memory.

## Performance & Optimization

[ImagePDF.tools](https://imagepdf.tools) is optimized for sub-second execution on standard hardware:

* **WebAssembly binaries**: Resource-intensive tasks (like JPEG/WebP compression and PDF editing) run on compiled WASM modules for near-native CPU speeds.
* **Client-side rendering**: Page layouts and image conversions leverage modern browser APIs (such as Canvas and FileReader) to avoid server bottlenecks.
* **Minimal bundle size**: Built on Next.js 16 with optimized chunks, ensuring instant initial page loads and smooth routing.

## Project Goals

* Become the most intuitive, privacy-respecting PDF and image utility on the web.
* Maintain a fast, lightweight, and responsive workspace.
* Expand the collection of tools based on community requests.
* Retain a simple, focused interface that gets out of the user's way.

## Star the Repository

If you find [ImagePDF.tools](https://imagepdf.tools) helpful, please star this repository. Your support helps others discover the project and encourages further development.
