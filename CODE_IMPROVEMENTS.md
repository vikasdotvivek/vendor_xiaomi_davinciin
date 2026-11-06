# Vendor snapshot review — 2026-11-06

The automated scanner has no rules for Android `.mk` build files or proprietary binaries. Its zero inspected-source count is a limitation, not evidence that the vendor software is correct or secure. Manually inspected: `BoardConfigVendor.mk`, the first 95 lines of `Android.mk`, and the inventory/exclusions. No binaries were run, modified, or reverse engineered.

## Appropriate improvements for this repository

- **Preserve generated-file provenance.** Both inspected makefiles identify `device/xiaomi/davinciin/setup-makefiles.sh` as their generator. Document the exact device-tree revision, generator revision and extraction command so these files can be regenerated rather than manually patched without provenance.
- **Record blob provenance and integrity.** Add a manifest of the source firmware build/fingerprint and file hashes. Validate extraction against that manifest before publishing a replacement snapshot. Do not infer binary security from filenames or age.
- **Check declared input files.** Add a non-executing check that each `LOCAL_SRC_FILES`, `LOCAL_SRC_FILES_32` and `LOCAL_SRC_FILES_64` entry resolves to the expected file and architecture. The inspected declarations deliberately include both 32-bit and 64-bit libraries; preserve that distinction.
- **Validate in the intended Android build.** The `TARGET_DEVICE` gate names `davinciin`. Record the compatible device tree, Android/LineageOS branch and build command; test image integration there instead of applying unrelated upstream source fixes.
- **Keep licensing and redistribution provenance explicit.** Do not treat the makefiles' license header as a license for every proprietary blob. Maintain the relevant extraction and redistribution documentation.

This is a provenance/build-integrity improvement plan, not a binary vulnerability audit. The generated code and proprietary payloads remain unchanged on this review branch.
