# Performance Verdict: PaddleOCR-VL (v1.6) for Math OCR

**Important Note:** This performance evaluation is strictly isolated to the model's capabilities regarding mathematical OCR tasks.

---

### Executive Summary

Based on baseline testing of mathematical expressions, PaddleOCR-VL (v1.6) demonstrates a clear divergence in capability depending entirely on the source formatting. The model delivers exceptional, production-grade accuracy when parsing digital math fonts and printed textbook equations. However, it lacks robust spatial boundaries when dealing with unstructured handwritten math, creating distinct layout anomalies that require explicit algorithmic overrides or specialized reasoning safeguards.

---

### Printed Font Math OCR Performance

The model is exceptionally reliable when processing clean, printed mathematical document layouts. It effortlessly maps intricate typographic markers, vectors, derivatives, summations, and Greek symbols directly into clean, compilable LaTeX syntax. Furthermore, when presented with dense textbook equation arrays or formula lists, the layout processor naturally organizes the structural text using proper LaTeX alignment blocks instead of outputting disjointed, fractured blocks.

---

### Handwritten Math OCR Layout Flaws

While individual character recognition of handwriting remains highly accurate, the underlying layout parser struggles with spatial interpretation when handling handwritten equations. 

A primary flaw is horizontal token fusion caused by missing line separators. When processing handwritten equations stacked vertically, the layout engine frequently omits critical line breaks or space delimiters. This causes independent numbers and variables from entirely different rows to smash together into a single ambiguous string, such as fusing a trailing number from a top equation with a leading variable from a bottom equation.

Additionally, the engine suffers from reading orientation inversion. Without explicit task prompting, the layout engine can misjudge the vertical orientation of loose handwriting. This causes the model to occasionally transcribe the bottom line of a handwritten mathematical derivation before reading the top line, ultimately passing an inverted sequence of equations to the subsequent reasoning engine.