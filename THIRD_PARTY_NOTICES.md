
# Third-Party Notices

ThumbLLM is developed and distributed by TeksEdge.

ThumbLLM includes, incorporates, or distributes software developed by third parties. Those third-party components remain the property of their respective authors and contributors and are distributed under their respective licenses.

Nothing in this file changes the license or ownership of ThumbLLM itself.

Full copies of applicable third-party license texts are provided in the `licenses/` directory accompanying this distribution.

---

## llama.cpp / ggml

ThumbLLM includes and launches the `llama-server` runtime from the llama.cpp project.

**Project:** llama.cpp
**Project URL:** https://github.com/ggml-org/llama.cpp
**License:** MIT License

This ThumbLLM CPU release was built around the official Windows x64 CPU llama.cpp distribution:

**llama.cpp build:** b10603
**Commit:** c060ca974

The complete llama.cpp license is provided at:

`licenses/LICENSE-llama.cpp.txt`

llama.cpp and ggml remain copyright of their respective authors and contributors.

---

## cpp-httplib

llama.cpp's server functionality incorporates cpp-httplib.

**Project:** cpp-httplib
**License:** MIT License

The complete license is provided at:

`licenses/LICENSE-cpp-httplib.txt`

cpp-httplib remains copyright of its respective authors and contributors.

---

## nlohmann/json

llama.cpp incorporates the nlohmann/json JSON library.

**Project:** JSON for Modern C++ / nlohmann/json
**License:** MIT License

The complete license is provided at:

`licenses/LICENSE-nlohmann-json.txt`

nlohmann/json remains copyright of its respective authors and contributors.

---

## LLVM OpenMP Runtime

The Windows x64 CPU llama.cpp runtime distributed with this ThumbLLM release uses the LLVM OpenMP runtime.

**Component:** LLVM OpenMP / libomp
**License:** Apache License 2.0 with LLVM Exceptions

The complete applicable LLVM license and exception text is provided at:

`licenses/LICENSE-LLVM.txt`

LLVM OpenMP remains copyright of its respective authors and contributors.

---

## BoringSSL

The llama.cpp Windows CPU runtime used by this ThumbLLM release was built with BoringSSL support.

**Project:** BoringSSL
**License:** Apache License 2.0 and additional notices contained in the BoringSSL distribution

BoringSSL contains code originating from or derived from multiple projects. The complete BoringSSL license file, including its applicable third-party notices, is provided at:

`licenses/LICENSE-BoringSSL.txt`

BoringSSL and its incorporated components remain copyright of their respective authors and contributors.

---

## CPython

ThumbLLM is implemented using Python and the standalone Windows executable contains portions of the CPython runtime required to execute the application.

**Project:** CPython
**License:** Python Software Foundation License Version 2 and additional historical licenses included with the applicable Python distribution

The complete license file from the Python interpreter used to build this ThumbLLM release is provided at:

`licenses/LICENSE-Python.txt`

Python and CPython remain copyright of the Python Software Foundation and their respective contributors.

---

## Tcl/Tk

ThumbLLM's desktop interface uses Python's `tkinter` interface to Tcl/Tk. The packaged application therefore includes Tcl/Tk components required to operate the graphical user interface.

**Project:** Tcl/Tk
**License:** Tcl/Tk License

The applicable Tcl/Tk license terms from the Tcl/Tk version distributed with this ThumbLLM build are provided at:

`licenses/LICENSE-TclTk.txt`

Tcl/Tk remains copyright of its respective authors and contributors.

---

## Public-Domain Components Used by llama.cpp

llama.cpp incorporates or uses several components that are distributed as public-domain software or under equivalent permissive terms.

These may include:

* stb_image
* miniaudio
* subprocess.h

These components are acknowledged here for completeness. Their inclusion does not impose additional licensing terms on ThumbLLM beyond the terms applicable to those individual components.

---

# Model Notice

The language-model weights used by this ThumbLLM edition are **not embedded in or distributed as part of the ThumbLLM executable**.

When the required model is not already present on the user's computer, ThumbLLM downloads it separately from its model repository.

This edition is configured for:

**Model:** OBLITERATUS/Ornith-1.5-9B-OBLITERATED
**Quantization:** Q4_K_M
**Model license:** MIT
**Upstream model:** ornith-ai/Ornith-1.5-9B
**Architecture lineage:** Qwen3.5

The downloaded model and its associated files are governed by the licenses and terms published by their respective model authors and repositories.

Because the model weights are downloaded separately rather than redistributed inside this ThumbLLM release, their licenses are not reproduced as licenses for the ThumbLLM executable itself.

If a future ThumbLLM distribution includes model weights directly — for example, on physical media or inside a bundled download — the applicable model and upstream license notices must accompany that distribution.

---

# No Endorsement

References to third-party projects, model developers, organizations, or trademarks are provided solely for attribution and identification.

Their inclusion does not imply sponsorship, endorsement, or affiliation with ThumbLLM or TeksEdge.

---

# Additional Information

Third-party components may be updated between ThumbLLM releases.

The license files accompanying each ThumbLLM release correspond to the components distributed with that specific release.

For source code, license information, and additional details about the projects identified above, consult their respective upstream repositories.
