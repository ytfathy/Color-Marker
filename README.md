# Honolulu B Color Match — Ohuhu 320 Marker Reference

An interactive, browser-based color utility designed for artists, illustrators, and colorists using the **Ohuhu Honolulu B (320 Marker Set)**. 

This tool eliminates the guesswork when selecting or matching markers by allowing you to match target hex codes or sampled image colors against all **200 distinct markers** contained in the Honolulu B catalog using perceptually accurate color math.

---

## Key Features

*   **Hex Code Matching**: Input any 6-digit hex code (`#3E8969` or `3e8969`) to verify whether you own an exact color match in your Honolulu B set. If an exact match isn't present, the tool finds the closest alternatives available on your desk.
*   **Perceptual Color Science ($\Delta E$ / CIE76)**: Color distance is calculated using **$\Delta E$ (CIE76)** in the **$L^*a^*b^*$ color space**, rather than standard Euclidean RGB distances. This accurately reflects human eye perception when measuring color differences.
*   **Photo Eyedropper & Magnifier**: Upload any reference image or artwork, hover over pixels with an interactive magnifying glass, and sample colors directly to find marker equivalents.
*   **Custom Palette Builder**:
    *   Save selected marker colors into custom palettes (e.g., *"Skin Tones"*, *"Botanical"*).
    *   Export and import saved palettes via structured JSON files for backup or sharing across devices.
*   **Palette Image Previewer**: Upload an image, select one of your custom marker palettes, and render a preview showing how your image looks when re-mapped exclusively to those marker shades. Download the generated swatch preview directly.
*   **Full Catalog Explorer**: Browse all 200 Honolulu B markers categorized by official Ohuhu color families (*Red/Pink/Magenta*, *Yellow/Orange/Peach*, *Earth/Brown*, *Green/Yellow-Green*, *Blue/Teal/Blue-Violet*).

---

## Technical Architecture & Implementation

The project is built as a single, zero-dependency HTML file containing responsive CSS and vanilla JavaScript.

### Core Stack
*   **Markup & Layout**: Semantic HTML5 with flexible CSS grid/flexbox.
*   **Typography**: Inter, Fraunces, and JetBrains Mono via Google Fonts.
*   **Rendering**: HTML5 Canvas API for real-time pixel sampling, magnification, and palette re-mapping rendering.
*   **Storage**: Client-side browser `localStorage` for saving custom palettes across user sessions.

### Color Matching Pipeline
1. **Hex to RGB**: Parses input strings into normalized $RGB \in [0, 1]^3$.
2. **RGB to CIE $L^*a^*b^*$**:
   - Converts $RGB$ to standard $XYZ$ color space using the $D_{65}$ illuminant reference white.
   - Transforms $XYZ$ to non-linear $L^*a^*b^*$ coordinates representing lightness ($L^*$), green-red axis ($a^*$), and blue-yellow axis ($b^*$).
3. **CIE76 Delta-E Calculation**: Calculates color difference ($\Delta E^*_{ab}$) between the target color ($L_1^*, a_1^*, b_1^*$) and candidate markers ($L_2^*, a_2^*, b_2^*$) using standard Euclidean distance in $L^*a^*b^*$ space:

$$\Delta E^*_{ab} = \sqrt{(L_2^* - L_1^*)^2 + (a_2^* - a_1^*)^2 + (b_2^* - b_1^*)^2}$$

*   $\Delta E \le 1.0$: Difference is visually imperceptible to human eyes.
*   $\Delta E \le 3.0$: Close match; noticeable only on close inspection.
*   $\Delta E > 5.0$: Noticeably different color shade.

---

## Getting Started

Because this application runs natively in the browser without external build tools or backend servers:

1. Clone or download the repository:
   ```bash
   git clone [https://github.com/your-username/honolulu-b-color-match.git](https://github.com/your-username/honolulu-b-color-match.git)
