## [1.0.4](https://github.com/stackgobrr/common-github-actions/compare/v1.0.3...v1.0.4) (2026-01-26)


### Bug Fixes

* correct GitHub Actions input syntax and format terraform files ([64e154a](https://github.com/stackgobrr/common-github-actions/commit/64e154aded5c5706832b721b2732c5ba2e8b67a1))

## [1.0.3](https://github.com/stackgobrr/common-github-actions/compare/v1.0.2...v1.0.3) (2026-01-25)


### Bug Fixes

* moving var file flag to plan cmd ([5871e44](https://github.com/stackgobrr/common-github-actions/commit/5871e4425ac661ae77060e7cce0cd327dc67a959))
* s3 state file name ([be509de](https://github.com/stackgobrr/common-github-actions/commit/be509de533b59e68f0117ba18e4e9e5d6eb9bbc7))
* tf var file flag ([b765f9c](https://github.com/stackgobrr/common-github-actions/commit/b765f9c2c8e2ce3332c6cd70d7cd785dae0c7265))

## [1.0.2](https://github.com/h3ow3d/h3ow3d-github-actions/compare/v1.0.1...v1.0.2) (2025-12-22)


### Bug Fixes

* **workflows:** add retry logic and caching for TFLint setup to handle 503 errors ([9ed4f9a](https://github.com/h3ow3d/h3ow3d-github-actions/commit/9ed4f9a822426d4eb6f2e2af79d7a4de5644bcd3))

## [1.0.1](https://github.com/h3ow3d/h3ow3d-github-actions/compare/v1.0.0...v1.0.1) (2025-12-22)


### Bug Fixes

* **workflows:** pass GITHUB_TOKEN as input parameter to tfsec-action to prevent API rate limiting ([824ffb2](https://github.com/h3ow3d/h3ow3d-github-actions/commit/824ffb265cef9782f1e1fe9a042d8e49f7af68b5))

## 1.0.0 (2025-12-22)


### Features

* add automated semantic release workflow ([ab66b75](https://github.com/h3ow3d/h3ow3d-github-actions/commit/ab66b759158dfc210ef020de508ecea7cfd32c69))
* add AWS credentials support for terraform plan in CI ([f83f51e](https://github.com/h3ow3d/h3ow3d-github-actions/commit/f83f51e3aec0e0d40dafc8d4fa753c879b928b7e))
* add reusable composite actions for Node.js, Terraform, and AWS ([bfbd022](https://github.com/h3ow3d/h3ow3d-github-actions/commit/bfbd022274ca8d1d387dc07a1cc96b55acae3104))
* add reusable Terraform module workflows [release] ([2618c48](https://github.com/h3ow3d/h3ow3d-github-actions/commit/2618c481bac8ba0f3d38d7a4fed178d9d21a65d1))
* add reusable Terraform workflows [release] ([d460fd4](https://github.com/h3ow3d/h3ow3d-github-actions/commit/d460fd4a19560a3bb85777c4b18ac381bcaac6bb))


### Bug Fixes

* remove npm cache when no lockfile specified ([4e885c1](https://github.com/h3ow3d/h3ow3d-github-actions/commit/4e885c1c63b86e12a96e73f3f4989d77484c0ecc))
* remove terraform plan from CI to avoid AWS credential requirement ([bd09b44](https://github.com/h3ow3d/h3ow3d-github-actions/commit/bd09b44dc0978635845c3735e9f924640866cad3))
* set HUSKY=0 in install step to skip git hooks in CI ([43413a7](https://github.com/h3ow3d/h3ow3d-github-actions/commit/43413a7b9e2a0f7ed2c1f59f97f50f9630777015))
