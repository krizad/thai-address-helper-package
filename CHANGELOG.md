# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-06-18

### Added

- Initial release of `@krizad/thai-address-helper`
- In-memory Thai address lookup engine (no API required)
- `searchAllFields()` — search across sub-district, district, province, and zipcode
- `searchBySubDistrict()`, `searchByDistrict()`, `searchByProvince()`, `searchByZipcode()` — field-specific search
- `SearchLevel` parameter on all search functions to return targeted data (`province`, `district`, `subDistrict`, `all`)
- English language search support (`subDistrictEng`, `districtEng`, `provinceEng`)
- Cascading dropdown support:
  - `getDropdownList()` — unified function for province/district/subDistrict dropdowns
  - `getUniqueProvinces()`, `getDistrictsByProvince()`, `getSubDistrictsByDistrict()`
  - `getZipcodeByHierarchy()` — auto-fill zipcode from selected hierarchy
  - `getUniqueZipcodes()`
  - `cascadeFromZipcode()`, `cascadeFromSubDistrict()` — reverse cascade for auto-complete forms
- `formatToDropdownOptions()` — format addresses for UI dropdown components (e.g., React-Select)
- Full TypeScript support with `ThaiAddress`, `SearchLevel`, and `DropdownOption` types
- Dual CJS/ESM export via `tsup`
- Zero runtime dependencies
- Node.js >= 18 support