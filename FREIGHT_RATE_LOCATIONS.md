# Freight Rate Configuration - Single Source of Truth

## Current Freight Rate: $20.00 per cubic foot

This document lists ALL locations where the freight rate is defined in the codebase.
When changing the freight rate in the future, update ALL of these locations.

## Backend Files

### 1. `/backend/pricing.js`
- Line 3: `FREIGHT_RATE_PER_CUFT: 20.00`
- Used by: feeCalculator.js
- Purpose: Main pricing constant export

### 2. `/backend/pricing/index.js`
- Line 15: `const FREIGHT_RATE_PER_CUFT = 20.00;`
- Line 16: `const SHIPPING_RATE_PER_CUBIC_FOOT = 20.00;`
- Purpose: Pricing calculation engine

### 3. `/backend/lib/freightEngine.js`
- Line 5: `const RATE_PER_FT3 = toNum(process.env.FREIGHT_RATE_PER_FT3) || 20;`
- Purpose: Freight estimation with buffer calculations

### 4. `/backend/routes/admin.js`
- Line 224: `const oceanRate = Number(process.env.OCEAN_RATE_PER_FT3 || 20);`
- Purpose: Admin API ping endpoint

### 5. `/backend/fastScraper.js`
- Line 45: `const OCEAN_RATE_PER_FT3 = Number(process.env.OCEAN_RATE_PER_FT3 || 20);`
- Purpose: Web scraper pricing calculations

### 6. `/backend/utils/pricing.js`
- Line 47: `const rate = Number(opts.ratePerFt3 ?? process.env.OCEAN_RATE_PER_FT3 ?? 20);`
- Line 83: `ratePerFt3 = Number(process.env.OCEAN_RATE_PER_FT3 ?? 20),`
- Purpose: Shared pricing utilities

### 7. `/backend/utils/feeCalculator.js`
- Line 14: `FREIGHT_RATE_PER_CUFT: 20.00,` (emergency fallback)
- Line 15: `SHIPPING_RATE_PER_CUBIC_FOOT: 20.00,` (emergency fallback)
- Line 31: `?? 20.00` (final fallback)
- Purpose: Shipping & handling calculator

### 8. `/shared/pricing.js`
- Line 23: `const rate = Number(opts.ratePerFt3 ?? (process.env.OCEAN_RATE_PER_FT3 || 20.0));`
- Line 55: `ratePerFt3 = Number(process.env.OCEAN_RATE_PER_FT3 ?? 20.0),`
- Purpose: Shared pricing utilities used by frontend and backend

## Frontend Files

### 9. `/frontend/admin-calculator.html`
- Line 610: Display text `$<span id="currentRate">20.00</span>/ft³`
- Line 651: `let freightRatePerCuft = 20.00;` (JavaScript variable)
- Line 655: `freightRatePerCuft = value || 20.00;` (update function)
- Line 828: Placeholder `placeholder="20.00"`
- Line 830: Suggestion text `Using global: $20.00/ft³`
- Purpose: Admin calculator UI and calculations

## Environment Variable

The freight rate can be overridden via environment variable:
- `OCEAN_RATE_PER_FT3=20.00` in `.env` file

## How to Change the Freight Rate

1. Update the value in `.env` file: `OCEAN_RATE_PER_FT3=XX.XX`
2. OR update all 9 files listed above (if changing the default)
3. Restart the server to pick up the new value
4. Clear browser cache if the frontend still shows old values

## Notes

- All backend files fall back to 20.00 if no environment variable is set
- Frontend reads from the global JavaScript variable `freightRatePerCuft`
- The minimum freight charge is always $30.00 regardless of rate
- Individual products can override the global rate in the admin calculator
