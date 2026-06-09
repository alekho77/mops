# ADR-0008: Noncommercial source-available license

## Status: Accepted

## Context

MOPS is intended to be publicly readable and usable for personal and noncommercial purposes.

The repository should allow users to:

- read the source code;
- download and build the applications;
- use the system for personal purposes;
- use the system in noncommercial contexts.

The repository should not allow commercial use without explicit permission from the copyright holder.

The selected license must make commercial use require a separate written permission or commercial license from the rightsholder.

## Decision

Use **PolyForm Noncommercial License 1.0.0** for the MOPS repository.

Store the full license text in `LICENSE.md`.

Add the project copyright notice as a required notice:

```text
Required Notice: Copyright 2026 Aleksei Khozin (https://github.com/alekho77)
```

Commercial use of MOPS or derivative commercial products based on MOPS requires separate written permission or a separate commercial license from Aleksei Khozin.

Accept that this makes the repository **source-available / noncommercial**, not OSI-approved open source.

## Consequences

- Personal and noncommercial use is explicitly permitted.
- Users may read, download, build, modify, and distribute the software only under the license terms.
- Commercial use is prohibited without separate written permission or a commercial license.
- The repository is not open source under the Open Source Definition because field-of-use restrictions are present.
- Companies and commercial projects may avoid using the repository unless a commercial license is negotiated.
- Redistributors must keep the license terms and required notice with copies of the software.
- Future dependency and contribution decisions must preserve compatibility with the selected noncommercial licensing model.
