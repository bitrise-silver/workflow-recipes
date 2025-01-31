# (iOS) Cache Fastlane actions

## Description

Cache the `derived_data_path` folder set by Fastlane actions with the new key-based caching Steps, **Save Cache** and **Restore Cache**.

## Instructions

1. Add the [Restore Cache](https://bitrise.io/integrations/steps/restore-cache) Step to the Workflow.
2. Add Fastlane step, such as [Xcode Test for iOS](https://www.bitrise.io/integrations/steps/xcode-test). Sample content in Fastfile

```
scan(
    project: "DemoUITest.xcodeproj",
    scheme: "DemoUITest",
    derived_data_path: "fastlane_cache",
    build_for_testing: true,
    skip_build: true,
    device: "iPhone 15 (17.0.1)"
)
```

3. Add the [Save Cache](https://bitrise.io/integrations/steps/save-cache), make sure the value of `paths` parameter matches what is specified in Fastlane config `derived_data_path` in Fastfile



The generic [Restore Cache](https://bitrise.io/integrations/steps/restore-cache) and [Save Cache](https://bitrise.io/integrations/steps/save-cache) Steps should use the optimal keys so that cache reuse can be maximized while cache size is minimized.

## bitrise.yml

```yaml
- restore-cache@1:
    inputs:
    - key: fastlane-cache-{{.Branch}}
- fastlane@3:
    description: this step assumes that cache folder is fastlane_cache generated by Fastlane using derived_data_path parameter in Fastfile file
    inputs:
    - lane: build
- save-cache@1: {}
    inputs:
    - paths: fastlane_cache
    - key: fastlane-cache-{{.Branch}}
```
