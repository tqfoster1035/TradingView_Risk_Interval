# Changelog - Risk Interval Derivatives Indicator

All notable changes to this project will be documented in this file.

---

## [3.2.0] - 2025-11-22 - Official Rocket Scooter RI Values

### Changed
- **Switched from calculated to official RI values**
  - Now using verified Rocket Scooter RI Prime values directly
  - **ES (MESZ25):** 58.75 points (official, was ~58.70 calculated)
  - **NQ (MNQZ25):** 211.25 points (official, was ~210.51 calculated)
  - Eliminates calculation approximations and guesswork

- **Updated calculation method** (lines 72-87)
  - Removed formula-based calculation: `RI = (Initial Margin × Circuit Breaker %) / Point Value`
  - Now uses hardcoded official values from Rocket Scooter platform
  - Source: Matt Cowart's official Rocket Scooter platform values

### Documentation
- Updated README with official RI values
- Revised quarterly maintenance procedure (check Rocket Scooter for new values)
- Updated technical details section to reflect official sourcing
- Updated header comments to reference official values

---

## [3.1.0] - 2025-11-22 - Extended Derivative Levels

### Added
- **Three new derivative levels** for finer-grained price movement analysis
  - **3× RI** - Very strong directional moves (between 4× and 2×)
  - **1.5× RI** - Intermediate significant moves (between 2× and 1×)
  - **¾× RI** - Minor significant levels (between 1× and ½×)

- **Total levels now: 9** (previously 6)
  - Complete order: 4×, 3×, 2×, 1.5×, 1×, ¾×, ½×, ¼×, ⅛× RI

### Changed
- **Visibility toggles** - Added controls for new levels
- **Label toggles** - Added label controls for new levels
- **Alert toggles** - Added alert controls for new levels
- **Updated calculations** - Added upper/lower calculations for 3 new levels
- **Updated plots** - Added 6 new plot statements (3 levels × 2 directions)
- **Updated labels** - Added price labels for new levels
- **Updated info table** - Alert status now includes new levels
- **Updated alert logic** - Complete crossover/crossunder detection for new levels

### Documentation
- Updated README.md with new level counts and recommendations
- Updated recommended alert settings: Enable 4×, 3×, 2×, 1.5×, 1× for significant moves
- Updated code structure line numbers to reflect additions
- Updated version history across all documentation files

---

## [3.0.0] - 2025-11-22 - Code Reorganization & ES/NQ Focus

### Changed
- **Complete code reorganization** for logical flow and maintainability
  - All derivative levels ordered consistently (expanded to 9 levels in v3.1.0)
  - Original v3.0 order: 4× → 2× → 1× → ½× → ¼× → ⅛×
  - Current v3.1 order: 4× → 3× → 2× → 1.5× → 1× → ¾× → ½× → ¼× → ⅛×
  - Calculations, plots, labels, and alerts all follow same order
  - Enhanced section comments for clarity
  - Removed orphaned code blocks (4×RI was previously at end of file)

- **Formalized ES/NQ-only scope**
  - Removed GC, CL, NG, BTC instrument support (simplification)
  - Auto-detection now binary: ES or NQ
  - Manual override options simplified to ES/NQ only
  - Updated all documentation to reflect focused scope

### Documentation
- Added comprehensive header comments explaining purpose and scope
- Inline comments clarify update procedures for quarterly margin changes
- Alert recommendation comments updated for ES/NQ specifics

---

## [2.0.0] - 2025-11-22 - Auto-Detection & RI Recalibration

### Added
- **Auto-detection of chart instrument** (`risk_interval_derivatives.pine:9-27`)
  - Indicator automatically detects ES vs NQ based on chart symbol
  - Uses `syminfo.ticker` and `syminfo.root` for detection
  - Updates all parameters automatically when switching charts
  - No manual instrument selection required

- **Manual instrument override option**
  - New setting: "Manual Instrument Override" for edge cases
  - Allows forcing specific instrument if auto-detection fails
  - Grouped under "Instrument Settings" in inputs

- **Chart symbol display in info table**
  - New row showing current chart symbol
  - Shows whether using auto-detection or manual override
  - Format: "ES" or "ES (Manual)"

### Changed - RI Value Recalibration

**Context:** Original 2022 indicator values were empirically tested. Updated to match current margin requirements while maintaining empirically-validated circuit breaker percentages.

#### ES (E-mini S&P 500)
- **Circuit Breaker %:** 13.0% (theoretical) → **13.7%** (empirically-tested)
- **Calculated RI:** ~**58.70** points
- **Reasoning:** 13.7% produces values matching 2022 indicator (~58.75)
- **Current Margin:** $21,424

#### NQ (E-mini Nasdaq 100)
- **Circuit Breaker %:** 13.0% (theoretical) → **13.7%** (empirically-tested)
- **Calculated RI:** ~**210.51** points
- **Reasoning:** 13.7% produces values matching 2022 indicator (~210.25)
- **Current Margin:** $30,732

---

## [1.0.0] - 2025-11-22 - Initial Multi-Instrument Version

### Features
- Support for 6 instruments (ES, NQ, GC, CL, NG, BTC)
- Formula derivation: `RI = (Initial Margin × Circuit Breaker %) / Point Value`
- Derivative level calculations: 4×, 2×, 1×, ½×, ¼×, ⅛×
- Granular alert controls per level
- Price labels on each level
- Information table showing parameters
- Manual RI override for rollovers
- Previous close anchor (updates daily)

### Research Findings
- ES/NQ use 13.7% CB (empirically-tested, theoretical 13% Level 2 NYSE halt)
- GC uses 9% CB (DCB 10% variant)
- NG/BTC use 10% CB (DCB variant)
- CL identified as anomaly (formula doesn't apply)

**Note:** v1.0 supported multiple instruments but was simplified in v2.0+ to focus on ES/NQ only.

---

## Technical Details

### Formula
```
RI = (Initial Margin × Circuit Breaker %) / Point Value
```

### Current Values (ES/NQ Only)

| Instrument | RI Value | Init Margin | Point Value | CB % |
|------------|----------|-------------|-------------|------|
| ES         | ~58.70   | $21,424     | $50         | 13.7% |
| NQ         | ~210.51  | $30,732     | $20         | 13.7% |

### Why 13.7% for ES/NQ?

- Theoretical NYSE Level 2 circuit breaker is **13%**
- Empirical testing from 2022 indicator showed values ~58.75 (ES) and ~210.25 (NQ)
- **13.7% produces values matching empirical results**
- Likely proprietary adjustment by original creator (Matt Cowart)

---

## Quarterly Maintenance

### When CME Updates Margins (Mar/Jun/Sep/Dec)

1. **Check new margins:** https://www.cmegroup.com/trading/price-limits.html
2. **Update code:** Lines 61-70 in `risk_interval_derivatives.pine`
   - ES: Update `initialMargin` value (line 63)
   - NQ: Update `initialMargin` value (line 68)
3. **Leave constant:** `pointValue` and `circuitBreakerPct` remain unchanged
4. **Alternative:** Use "Manual RI Override" setting (temporary solution)

### Example Update
```pinescript
if instrumentSelect == "ES"
    initialMargin := 22000.0    // ← Update this value only
    pointValue := 50.0          // ← Keep constant
    circuitBreakerPct := 0.137  // ← Keep constant
```

New RI calculation: (22,000 × 0.137) / 50 = **60.28 points**

---

## Migration Notes

### From Old 7-Instance Manual Setup

**Before:**
- Load old "Risk Interval" indicator 7 times
- Manually input different RI values (4×, 2×, 1×, ½×, ¼×, ⅛×)
- Manual changes when switching instruments
- Per-instance alert configuration

**After:**
- Single indicator instance
- All derivative levels calculated automatically
- Auto-detects ES/NQ from chart
- Granular alert controls per level
- Built-in visibility toggles
- Manual override available if needed

---

## Testing Checklist

### Verify After Loading

- [ ] Load indicator on ES chart
- [ ] Verify Base RI shows ~58.70
- [ ] Verify all derivative levels display correctly
- [ ] Switch to NQ chart
- [ ] Verify auto-detection updates to NQ
- [ ] Verify Base RI updates to ~210.51
- [ ] Test manual override toggle
- [ ] Configure alert levels (recommended: 4×, 2×, 1× only)
- [ ] Verify info table displays correct parameters

---

## Version History Summary

- **v3.2.0:** Using official Rocket Scooter RI Prime values (no more calculations)
- **v3.1.0:** Added 3 new derivative levels (3×, 1.5×, ¾× RI) - 9 total levels
- **v3.0.0:** Code reorganization, formalized ES/NQ-only scope
- **v2.0.0:** Auto-detection, RI recalibration to match empirical values
- **v1.0.0:** Initial multi-instrument version (6 instruments)

---

**Last Updated:** 2025-11-22
**Version:** v3.2.0
**Branch:** `claude/add-new-ri-levels-015C76GXdQCoeHYw97tuJWCs`
**Status:** Production ready - ES/NQ only - Official Rocket Scooter values
