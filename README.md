# Risk Interval Derivatives - ES/NQ Futures Indicator

**TradingView Pine Script indicator for E-mini S&P 500 (ES) and E-mini Nasdaq 100 (NQ) futures**

Automatically plots Risk Interval derivative levels (4×, 3×, 2×, 1.5×, 1×, ¾×, ½×, ¼×, ⅛×) based on CME margin requirements and circuit breaker thresholds. Predicts high-volume pivot points where margin risk intersects with halt thresholds.

---

## Quick Start

1. **Copy** `risk_interval_derivatives.pine` code
2. **Paste** into TradingView Pine Editor
3. **Add to chart** (ES or NQ)
4. **Configure alerts** (recommended: 4×, 3×, 2×, 1.5×, 1× for significant moves)

The indicator auto-detects ES/NQ and updates parameters automatically when switching charts.

---

## What is Risk Interval?

Risk Interval (RI) is a mathematical calculation that identifies price levels where market structure creates natural pivot points:

```
RI = (Initial Margin × Circuit Breaker %) / Point Value
```

### Current Values
- **ES:** ~58.70 points (based on $21,424 margin, 13.7% CB, $50 point value)
- **NQ:** ~210.51 points (based on $30,732 margin, 13.7% CB, $20 point value)

### Why It Works

Risk Interval levels correlate with:
- CME circuit breaker thresholds (13% Level 2 halt)
- Algorithmic stop-loss clusters
- Institutional risk limits
- Market maker hedging thresholds

When price approaches RI levels, trading systems recognize proximity to critical thresholds, creating self-fulfilling support/resistance.

---

## Features

### Core Functionality
- ✓ **Auto-detection:** Identifies ES/NQ from chart symbol
- ✓ **Derivative levels:** 4×, 3×, 2×, 1.5×, 1×, ¾×, ½×, ¼×, ⅛× RI (9 total levels)
- ✓ **Daily anchor:** Levels calculated from previous close
- ✓ **Price labels:** Optional labels on each level
- ✓ **Info table:** Real-time parameters display

### Alert Controls
- ✓ **Granular toggles:** Enable/disable alerts per level
- ✓ **Noise filtering:** Disable noisy levels (¾×, ½×, ¼×, ⅛×)
- ✓ **Recommended:** Enable 4×, 3×, 2×, 1.5×, 1× for significant moves

### Customization
- ✓ **Visibility toggles:** Show/hide individual levels
- ✓ **Manual override:** Force instrument selection or RI value
- ✓ **Colors & styles:** Customizable appearance

---

## Usage

### Recommended Alert Settings (ES/NQ)

**Enable:**
- 4× RI (extreme moves, rare but significant)
- 3× RI (very strong directional moves)
- 2× RI (strong directional moves)
- 1.5× RI (intermediate significant moves)
- 1× RI (base level, frequent tests)

**Disable:**
- ¾×, ½×, ¼×, ⅛× RI (typically too noisy for ES/NQ)

### Quarterly Maintenance

CME updates margins typically in **March, June, September, December**.

**Update procedure:**
1. Check new margins: https://www.cmegroup.com/trading/price-limits.html
2. Edit `risk_interval_derivatives.pine` lines 61-70
3. Update only `initialMargin` values (keep `pointValue` and `circuitBreakerPct` constant)

**Or use Manual RI Override** in indicator settings (temporary solution).

---

## File Structure

```
TradingView_Risk_Interval/
├── risk_interval_derivatives.pine    # Main indicator code
├── CHANGELOG.md                       # Version history & technical details
├── RI_Indicator_Handoff_Summary.md   # Full research & formula derivation
└── README.md                          # This file
```

---

## Technical Details

### Formula Derivation

**Discovery:** Reverse-engineered from Matt Cowart's (Rocket Scooter) proprietary Risk Interval methodology.

**Core calculation:**
```
RI = (Initial Margin × Circuit Breaker %) / Point Value
```

**Why 13.7% (not 13%)?**
- Theoretical NYSE Level 2 circuit breaker = 13%
- Empirical testing showed 13.7% matches 2022 indicator values
- 13.7% produces ES ~58.70, NQ ~210.51 (matches observed pivots)

### Code Structure

Organized in logical order (4× → 3× → 2× → 1.5× → 1× → ¾× → ½× → ¼× → ⅛×):
1. **Lines 1-83:** Configuration & RI calculation
2. **Lines 84-165:** Settings (visibility, labels, alerts, colors)
3. **Lines 167-207:** Derivative level calculations (9 levels)
4. **Lines 209-305:** Plot statements
5. **Lines 307-447:** Price labels
6. **Lines 449-563:** Information table
7. **Lines 565-659:** Alert conditions

---

## Version History

- **v3.1.0** (2025-11-22): Added 3 new derivative levels (3×, 1.5×, ¾× RI) - 9 total levels
- **v3.0.0** (2025-11-22): Code reorganization, formalized ES/NQ-only scope
- **v2.0.0** (2025-11-22): Auto-detection, RI recalibration to empirical values
- **v1.0.0** (2025-11-22): Initial multi-instrument version (6 instruments)

See `CHANGELOG.md` for detailed version history.

---

## Research & Background

For complete formula derivation, circuit breaker research, and multi-instrument analysis (GC, CL, NG, BTC), see `RI_Indicator_Handoff_Summary.md`.

**Original methodology:** Matt Cowart, Founder/CEO Rocket Scooter & Rocket Place

---

## Support & Updates

**CME Resources:**
- Price Limits: https://www.cmegroup.com/trading/price-limits.html
- Margins: https://www.cmegroup.com/solutions/risk-management/performance-bonds-margins.html
- Circuit Breakers: https://www.cmegroup.com/solutions/market-access/globex/trade-on-globex/faq-dynamic-circuit-breakers.html

**Quarterly margin changes:**
Check CME announcements typically 1 week before contract expiration.

---

## License

Personal use. Original Risk Interval methodology by Matt Cowart (Rocket Scooter).

---

**Last Updated:** 2025-11-22
**Version:** v3.1.0
**Status:** Production ready - ES/NQ only
**Branch:** `claude/add-new-ri-levels-015C76GXdQCoeHYw97tuJWCs`
