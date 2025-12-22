# 📸 StudioLens: Raw-to-Studio Portrait Pipeline

**StudioLens** is a computer vision pipeline that transforms raw, uncontrolled human portraits (mobile-captured, low-light, or blurry) into professional, studio-quality images. It simulates high-end lighting and depth-of-field while preserving original facial identity.

---

## 🚀 1. Purpose
The project automates the "retouching" process by fixing common photography issues like sensor noise, motion blur, and distracting backgrounds, outputting a polished portrait that looks like it was shot with a professional DSLR in a studio.

## 🏗️ 2. Pipeline Architecture
The image flows through a sequential processing chain:
1. **Denoise:** `fastNlMeans` removes sensor grain.
2. **Lighting:** `CLAHE` (LAB space) balances uneven exposure.
3. **Restoration:** `MTCNN` detects faces and `GFPGAN` restores facial features.
4. **Segmentation:** `U²-Net` (via rembg) isolates the subject.
5. **Studio Bokeh:** Feathered Gaussian Blur creates a smooth subject-background transition.
6. **Final Polish:** Unsharp Masking adds micro-contrast.

## 📂 3. File Directory
├── main.py                # CLI entry point
├── enhance.py             # Pipeline orchestrator
├── utils.py               # Image & resizing helpers
├── pipeline/
│   ├── deblur_denoise.py  # Pre-processing modules
│   ├── face_restore.py    # AI Face reconstruction logic
│   ├── segmentation.py    # Background removal & Bokeh blending
└── weights/               # Local folder for model weights (GIT IGNORED)

## ✨ 4. Key Technical Features
---------------------------

*   **Models:** GFPGAN v1.4 (Facial Restoration), MTCNN (Face Detection), U²-Net (Salient Object Detection).
    
*   **Filters:** Adaptive Histogram Equalization (CLAHE) for relighting and Gaussian Feathering for realistic edge blending.
    
*   **Identity Preservation:** Uses alpha-blending logic to keep a balance between AI-restored pixels and original skin textures.
    

## 🛠️ 5. Installation & Requirements
----------------------------------

**Note:** Model weights are ignored by Git. You must set them up locally.

1.  codeBashpython -m venv venvsource venv/bin/activate # Windows: venv\\Scripts\\activatepip install -r requirements.txt
    
2.  **Model Weights (Required):**
    
    *   Download GFPGANv1.4.pth from the [official release](https://www.google.com/url?sa=E&q=https://github.com/TencentARC/GFPGAN/releases/download/v1.3.0/GFPGANv1.4.pth).
        
    *   Create a weights/ folder in the root directory and place the .pth file inside.
        

## 🖥️ 6. How to Run
-----------------

Run the pipeline via terminal using the following command:

codeBash

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   python main.py --input examples/input.jpg --output examples/output.jpg --face_restore_strength 0.35   `

*   \--face\_restore\_strength: Adjust from 0.0 to 1.0 (0.35 is recommended for natural results).
    
*   \--bokeh\_strength: Adjust the background blur radius.
    

## ✅ 7. Successfully Implemented Tasks
-----------------------------------

*   **Motion Blur Removal:** Successfully reconstructs features smeared by camera movement.
    
*   **Studio Bokeh:** Professional background isolation with soft-edge feathering.
    
*   **Low-Light Correction:** Brightens underexposed images while maintaining skin tones.
    
*   **Face Clarity:** Restores sharp eyes and facial details without the "waxy" AI look.
    
*   **Identity Maintenance:** 1:1 facial identity preserved across all test cases.
