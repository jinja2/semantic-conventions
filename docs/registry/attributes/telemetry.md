<!-- NOTE: THIS FILE IS AUTOGENERATED. DO NOT EDIT BY HAND. -->
<!-- see templates/registry/markdown/attribute_namespace.md.j2 -->

# Telemetry

## Telemetry Attributes

This document defines attributes for telemetry SDK.

| Attribute | Type | Description | Examples | Stability |
|---|---|---|---|---|
| <a id="telemetry-distro-name" href="#telemetry-distro-name">`telemetry.distro.name`</a> | string | The name of the auto instrumentation agent or distribution, if used. [1] | `parts-unlimited-java` | ![Development](https://img.shields.io/badge/-development-blue) |
| <a id="telemetry-distro-version" href="#telemetry-distro-version">`telemetry.distro.version`</a> | string | The version string of the auto instrumentation agent or distribution, if used. | `1.2.3` | ![Development](https://img.shields.io/badge/-development-blue) |
| <a id="telemetry-sdk-language" href="#telemetry-sdk-language">`telemetry.sdk.language`</a> | string | The language of the telemetry SDK. | `cpp`; `dotnet`; `erlang` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| <a id="telemetry-sdk-name" href="#telemetry-sdk-name">`telemetry.sdk.name`</a> | string | The name of the telemetry SDK as defined above. [2] | `opentelemetry` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| <a id="telemetry-sdk-version" href="#telemetry-sdk-version">`telemetry.sdk.version`</a> | string | The version string of the telemetry SDK. | `1.2.3` | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |

**[1] `telemetry.distro.name`:** Official auto instrumentation agents and distributions SHOULD set the `telemetry.distro.name` attribute to
a string starting with `opentelemetry-`, e.g. `opentelemetry-java-instrumentation`.

**[2] `telemetry.sdk.name`:** The OpenTelemetry SDK MUST set the `telemetry.sdk.name` attribute to `opentelemetry`.
If another SDK, like a fork or a vendor-provided implementation, is used, this SDK MUST set the
`telemetry.sdk.name` attribute to the fully-qualified class or module name of this SDK's main entry point
or another suitable identifier depending on the language.
The identifier `opentelemetry` is reserved and MUST NOT be used in this case.
All custom identifiers SHOULD be stable across different versions of an implementation.

---

`telemetry.sdk.language` has the following list of well-known values. If one of them applies, then the respective value MUST be used; otherwise, a custom value MAY be used.

| Value  | Description | Stability |
|---|---|---|
| `cpp` | cpp | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| `dotnet` | dotnet | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| `erlang` | erlang | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| `go` | go | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| `java` | java | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| `nodejs` | nodejs | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| `php` | php | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| `python` | python | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| `ruby` | ruby | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| `rust` | rust | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| `swift` | swift | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
| `webjs` | webjs | ![Stable](https://img.shields.io/badge/-stable-lightgreen) |
