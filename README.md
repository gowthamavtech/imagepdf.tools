<p align="center">
  <img src="https://raw.githubusercontent.com/your-username/imagepdf/main/public/icon.svg" alt="ImagePDF.tools Logo" width="96" height="96" />
</p>

<h1 align="center">ImagePDF.tools</h1>

<p align="center">
  <strong>Free browser-native document utilities. Private, offline-first, WebAssembly-powered.</strong>
</p>

<p align="center">
  <a href="https://imagepdf.tools">Visit ImagePDF.tools</a> |
  <a href="https://imagepdf.tools/privacy">Privacy Core</a> |
  <a href="https://imagepdf.tools/blog">Blog Guides</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License: MIT" />
  <img src="https://img.shields.io/badge/Privacy-Browser_Native-blue.svg" alt="Privacy: Browser-Native" />
  <img src="https://img.shields.io/badge/Built_with-Next.js-black.svg" alt="Built with Next.js" />
</p>

<hr />

[ImagePDF.tools](https://imagepdf.tools) is a collection of browser-native document utilities designed for quick, private, and frictionless file operations. It serves as a serverless, privacy-focused alternative to Smallpdf, iLovePDF, and TinyPNG. By executing all processing directly on your device via WebAssembly, canvas APIs, and client-side JavaScript, the platform guarantees that your files never leave your browser.

> [!IMPORTANT]
> **Zero File Transmission**: ImagePDF.tools executes all document operations in-memory on your local CPU. Your file data is never sent to our servers, logged, or cached. You retain 100% custody of your documents at all times.

## The Manifesto: Why We Built ImagePDF.tools

The modern web is built on a centralization model: you upload your personal data to remote servers, wait for virtual machines to process it, and pull the output back down. This architecture made sense in the early days of the web when web browsers were simple document viewers with limited computational power. Heavy operations, like rendering PDFs or optimizing raster images, had to run on high-performance server farms.

This server-dependent design created a massive utility industry. Popular platforms like Smallpdf, iLovePDF, and TinyPNG became standard workflows. However, this model has outlived its technical necessity. Modern computers and mobile devices possess massive CPU power, and browser technology has advanced with the introduction of WebAssembly (WASM). WebAssembly allows compiled languages like C, C++, and Rust to run directly inside browser sandbox environments at near-native speeds.

We built ImagePDF.tools to challenge the centralized processing model. We believe that basic document operations (such as converting a Word document, cropping an image, or merging PDF sheets) should be processed where they originate: locally on your device. By shifting the workload to the client, we remove the need for remote cloud architectures, eliminate network file latency, and resolve privacy and data custody concerns.

<h3 align="center">Comparison Matrix</h3>

<table width="100%">
  <thead>
    <tr>
      <th width="50%" align="left">Traditional Server Utilities (Smallpdf, iLovePDF)</th>
      <th width="50%" align="left">ImagePDF.tools Browser-Native Architecture</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Data Custody:</strong> Files are uploaded to external cloud servers, creating security and leakage risks.</td>
      <td><strong>Data Custody:</strong> Files stay in browser memory. Processing runs locally on your CPU.</td>
    </tr>
    <tr>
      <td><strong>Compliance:</strong> Potential compliance violations under GDPR, HIPAA, or CCPA due to server data storage.</td>
      <td><strong>Compliance:</strong> Zero regulatory exposure. No document contents are transmitted over networks.</td>
    </tr>
    <tr>
      <td><strong>Limits:</strong> Files size restrictions, queue times, and page caps to offset VM resource bills.</td>
      <td><strong>Limits:</strong> Infinite processing. No limits on file size, pages, or conversion frequencies.</td>
    </tr>
    <tr>
      <td><strong>Performance:</strong> Restricted by upload and download transfer speeds, especially on large assets.</td>
      <td><strong>Performance:</strong> Execution is instant. Network latency is completely bypassed.</td>
    </tr>
  </tbody>
</table>

## Why ImagePDF.tools is Free (And Better)

Traditional document toolkits operate on server-dependent structures. When you upload a file, they process it on their remote servers. This requires expensive virtual machines, database storage, and bandwidth. As their traffic grows, their cloud hosting bills become massive, forcing them to charge subscriptions, lock basic features behind paywalls, and cap file uploads.

ImagePDF.tools operates on a browser-native model:

* **Zero Marginal Cost**: Processing runs entirely on your local CPU via WebAssembly and Canvas APIs. Since we do not pay for remote servers to convert or compress your files, processing volume costs us nothing.
* **No File Limits**: Because you bring your own computational power, we can offer the service without file size limits, queues, or page count caps.
* **Instant Start**: You do not have to wait for uploads to complete or servers to queue your task. Conversions begin the moment you load the file.
* **Privacy by Design**: Traditional tools ask you to trust their deletion policies. We eliminate the risk completely by never transmitting your file content to any server.
* **Ad-Free Premium Option**: We monetize solely through a clean, optional ad-free plan, keeping all tools permanently free and accessible to everyone.

> [!TIP]
> **Pipeline Handoffs**: You can chain multiple tools together without downloading intermediate results. For example, crop a picture in the Image Editor and send it directly to the PDF Merger in a single click using our local in-memory Zustand handoff.

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

## Tool Index & Backlink Directory

Click any tool link below to access the utility directly on [ImagePDF.tools](https://imagepdf.tools):

### Core Tools Directory

* [All Image Tools](https://imagepdf.tools/image-tools): Complete collection of image resizing, cropping, and conversion utilities.
* [All PDF Tools](https://imagepdf.tools/pdf-tools): Suite of PDF merging, splitting, protection, and security tools.

### PDF Utilities

* [Merge PDF](https://imagepdf.tools/merge-pdf): Combine multiple PDF documents into a single file.
* [Split PDF](https://imagepdf.tools/split-pdf): Extract specific pages or divide a PDF into separate files.
* [Rotate PDF](https://imagepdf.tools/rotate-pdf): Adjust the orientation of individual PDF pages.
* [Number PDF](https://imagepdf.tools/number-pdf): Add page numbers to headers or footers in customizable formats.
* [Organize PDF](https://imagepdf.tools/organize-pdf): Reorder, insert, or delete pages visually.
* [Edit PDF](https://imagepdf.tools/edit-pdf): View, draw, and modify text annotations client-side.
* [Compress PDF](https://imagepdf.tools/compress-pdf): Optimize layout sizes locally without reducing quality.
* [Protect PDF](https://imagepdf.tools/protect-pdf): Encrypt files and set secure access passwords.
* [Unlock PDF](https://imagepdf.tools/unlock-pdf): Decrypt password-protected PDF files.
* [Watermark PDF](https://imagepdf.tools/watermark-pdf): Apply custom text or image overlays.
* [Rotate PDF Pages](https://imagepdf.tools/rotate-pdf): Rotate individual PDF sheets.

### Image Utilities

* [Resize Image](https://imagepdf.tools/resize-image): Modify physical image dimensions in pixels.
* [Crop Image](https://imagepdf.tools/crop-image): Trim margins or crop to specific aspect ratios.
* [Flip / Rotate Image](https://imagepdf.tools/flip-image): Flip horizontally, vertically, or rotate by custom degrees.
* [Reduce Image Size](https://imagepdf.tools/reduce-image-size): Shrink images with resolution-capping algorithms.
* [Compress Image](https://imagepdf.tools/compress-image): High-efficiency JPEG, PNG, and WebP size optimizer.
* [Compress JPEG](https://imagepdf.tools/compress-jpeg-online): MozJPEG WebAssembly compressor for photographs.
* [Compress PNG](https://imagepdf.tools/compress-png-online): UPNG.js palette quantization for diagrams and graphics.

### Document Conversion

* [Image to PDF](https://imagepdf.tools/image-to-pdf): Compile JPG, PNG, WebP, or SVG layouts into a single PDF document.
* [PDF to Image](https://imagepdf.tools/pdf-to-image): Render PDF pages into JPG images.
* [JPG to PDF](https://imagepdf.tools/jpg-to-pdf): Convert JPG photos to PDF layouts.
* [PDF to JPG](https://imagepdf.tools/pdf-to-jpg): Extract page canvases to JPEG format.
* [PDF to Word](https://imagepdf.tools/pdf-to-word): Reconstruct PDF elements into editable DOCX files.
* [Word to PDF](https://imagepdf.tools/word-to-pdf): Render DOCX files to PDF using Typst and Pandoc.
* [PNG to WebP](https://imagepdf.tools/png-to-webp): Convert transparent PNG files to high-performance WebP.
* [WebP to PNG](https://imagepdf.tools/webp-to-png): Convert WebP files to PNG images.
* [JPG to WebP](https://imagepdf.tools/jpg-to-webp): Convert JPG files to WebP.
* [WebP to JPG](https://imagepdf.tools/webp-to-jpg): Convert WebP files to JPG.
* [JPG to PNG](https://imagepdf.tools/jpg-to-png): Convert JPG files to PNG.
* [PNG to JPEG](https://imagepdf.tools/convert-png-to-jpeg): Convert PNG files to JPEG.

### Dedicated Single-Format Converters

* [JPEG to PNG Converter](https://imagepdf.tools/convert/jpeg-to-png): Convert JPEG photos to PNG files.
* [JPEG to WebP Converter](https://imagepdf.tools/convert/jpeg-to-webp): Convert JPEG files to WebP.
* [WebP to JPEG Converter](https://imagepdf.tools/convert/webp-to-jpeg): Convert WebP files to JPEG format.
* [SVG to PNG Converter](https://imagepdf.tools/convert/svg-to-png): Convert SVG vectors to PNG.
* [SVG to JPG Converter](https://imagepdf.tools/convert/svg-to-jpg): Convert SVG files to JPG.
* [SVG to JPEG Converter](https://imagepdf.tools/convert/svg-to-jpeg): Convert SVG files to JPEG.
* [SVG to WebP Converter](https://imagepdf.tools/convert/svg-to-webp): Convert SVG vectors to WebP.

### OCR & Metadata

* [Image to Text (OCR)](https://imagepdf.tools/image-to-text): Extract characters from images locally using Tesseract.
* [OCR PDF](https://imagepdf.tools/ocr-pdf): Create searchable PDF layouts with invisible text overlays.
* [Metadata Editor](https://imagepdf.tools/metadata-editor): Edit EXIF and GPS headers of JPEG photographs.
* [Remove Metadata](https://imagepdf.tools/remove-metadata): Strip EXIF headers from files with zero quality degradation.

### Informational & Support Pages

Explore our directory guides and policy references:

* [About Us](https://imagepdf.tools/about): Learn about our mission and client-side processing architecture.
* [Pricing Plans](https://imagepdf.tools/pricing): Compare our permanently free core features with the ad-free Pro subscription.
* [Support & FAQ](https://imagepdf.tools/support): Browse quick troubleshooting tips and reach out for assistance.
* [Contact Us](https://imagepdf.tools/contact): Get in touch with our open-source project maintainers.
* [Blog Index](https://imagepdf.tools/blog): Read guides on document privacy, compression quality, and WebAssembly compilation.
* [Privacy Core](https://imagepdf.tools/privacy): Overview of our browser-native private design.
* [Privacy Policy](https://imagepdf.tools/privacy-policy): Detailed breakdown of user authentication and billing storage.
* [Terms of Service](https://imagepdf.tools/terms): Review our licensing terms and guidelines.
* [Refund Policy](https://imagepdf.tools/refund): Check our subscription cancellation guidelines.
* [What's New](https://imagepdf.tools/whats-new): Discover recent tool updates, performance improvements, and bug fixes.

### Blog Directory (Knowledge Hub)

Read our tutorials and architectural insights directly on the website:

* [How to Check If an Online File Converter Is Safe](https://imagepdf.tools/blog/online-file-converters-security-risk)
* [How to Safely Compress Sensitive PDF Documents](https://imagepdf.tools/blog/safely-compress-sensitive-pdf-documents)
* [What is Image Metadata? EXIF and GPS Explained](https://imagepdf.tools/blog/what-is-image-metadata-exif-explained)
* [iLovePDF vs Smallpdf vs ImagePDF.tools Comparison](https://imagepdf.tools/blog/ilovepdf-vs-smallpdf-vs-imagepdf-tools-comparison)
* [Is iLoveIMG Safe? What Happens to Your Photos?](https://imagepdf.tools/blog/is-iloveimg-safe-what-happens-to-your-photos)
* [How to Password Protect a PDF Without Uploading](https://imagepdf.tools/blog/password-protect-pdf-without-uploading)
* [Best TinyPNG Alternative with No Upload Limits](https://imagepdf.tools/blog/best-tinypng-alternative-no-upload)
* [WebP vs AVIF Image Format for SEO in 2026](https://imagepdf.tools/blog/webp-vs-avif-image-format-seo-2026)
* [Core Web Vitals and Image Optimization Guide](https://imagepdf.tools/blog/core-web-vitals-image-optimization)
* [Ultimate Guide to PDF Compression Levels](https://imagepdf.tools/blog/ultimate-guide-pdf-compression-levels)
* [Convert JPG to PDF Without Losing Quality](https://imagepdf.tools/blog/convert-jpg-to-pdf-without-losing-quality)
* [How to Shrink a PDF for Email Attachments](https://imagepdf.tools/blog/shrink-pdf-for-email-attachments)
* [How to Batch Resize Images for Social Media](https://imagepdf.tools/blog/batch-resize-images-social-media)
* [PNG to WebP: Why Website Owners Should Switch](https://imagepdf.tools/blog/png-to-webp-why-website-owners-should-switch)
* [Building a Private Image Compressor with WebAssembly](https://imagepdf.tools/blog/building-private-image-compressor-webassembly)
* [How Client-Side Processing Saves Your Private Data](https://imagepdf.tools/blog/client-side-processing-saves-your-data)
* [Choosing the Right Image Format: Compression Guide](https://imagepdf.tools/blog/choosing-right-image-format-compression-guide)
* [How to Compress an Image to an Exact File Size](https://imagepdf.tools/blog/compress-image-to-exact-file-size)
* [Compression Artifacts: A Field Guide](https://imagepdf.tools/blog/compression-artifacts-field-guide)
* [How Modern Browsers Handle and Render Images](https://imagepdf.tools/blog/how-browsers-handle-images)
* [Image Compression for Absolute Beginners](https://imagepdf.tools/blog/image-compression-for-beginners)
* [Image Compression for Photographers](https://imagepdf.tools/blog/image-compression-for-photographers)
* [Image Optimization for E-Commerce Platforms](https://imagepdf.tools/blog/image-optimization-ecommerce)
* [The JPEG Quality Slider Explained](https://imagepdf.tools/blog/jpeg-quality-slider-explained)
* [Lazy Loading Images and the LCP Pitfall](https://imagepdf.tools/blog/lazy-loading-images-lcp-pitfall)
* [Lossy vs Lossless Image Compression](https://imagepdf.tools/blog/lossy-vs-lossless-compression)
* [Why You Can Trust Online Image Compression Tools](https://imagepdf.tools/blog/trust-online-image-compression-tools)
* [Progressive JPEGs: Optimization Guide](https://imagepdf.tools/blog/progressive-jpegs-optimization)
* [The Comprehensive Responsive Images Guide](https://imagepdf.tools/blog/responsive-images-guide)
* [Shopify Image Optimization Guide](https://imagepdf.tools/blog/shopify-image-optimization)
* [Why Images Are the Slowest Thing on Your Website](https://imagepdf.tools/blog/images-slowest-thing-on-your-website)
* [How to Crop Images Online Instantly](https://imagepdf.tools/blog/how-to-crop-images-online)
* [How to Batch Resize Images](https://imagepdf.tools/blog/how-to-batch-resize-images)
* [How to Watermark a PDF Online](https://imagepdf.tools/blog/how-to-watermark-a-pdf)
* [How to Convert HEIC to JPG](https://imagepdf.tools/blog/how-to-convert-heic-to-jpg)
* [5 Ways to Speed Up Your Image Workflow](https://imagepdf.tools/blog/5-ways-to-speed-up-image-workflow)

## Privacy Architecture

Privacy is not a feature promise: it is the structural architecture of [ImagePDF.tools](https://imagepdf.tools).

* **Zero upload**: Files are loaded directly into the browser and processed in-memory. They are never sent to external servers.
* **Optional account features**: User accounts (managed via Clerk) and billing (managed via Stripe) are only used to authenticate users and handle Pro plan status. These systems never touch your documents or images.
* **No data footprint**: Once you close the browser tab, all session data and processed files are cleared from your device memory.

## Star the Repository

If you find [ImagePDF.tools](https://imagepdf.tools) helpful, please star this repository. Your support helps others discover the project and encourages further development.
