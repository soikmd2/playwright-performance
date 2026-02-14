# Playwright-Performance Plugin - Feature Output Examples

This document provides visual examples of recommended features and their expected outputs.

**Fidelity Levels:**
- 🟢 **High Fidelity** - Production-ready example, exact format
- 🟡 **Medium Fidelity** - Conceptual design, format may vary
- 🟠 **Low Fidelity** - Illustrative concept, requires refinement

---

## Table of Contents

### Immediate Actions
1. [Baseline Comparison Feature](#1-baseline-comparison-feature)
2. [Median and Percentile Statistics](#2-median-and-percentile-statistics)
3. [Improved Error Messages and Validation](#3-improved-error-messages-and-validation)
4. [Performance Budget Enforcement](#4-performance-budget-enforcement)
5. [Unit Tests for Core Components](#5-unit-tests-for-core-components)

### Short Term
6. [Trend Analysis and Historical Reporting](#6-trend-analysis-and-historical-reporting)
7. [Enhanced HTML Charts with Interactivity](#7-enhanced-html-charts-with-interactivity)
8. [CI/CD Integration Helpers](#8-cicd-integration-helpers)
9. [Stream-Based Analysis for Scalability](#9-stream-based-analysis-for-scalability)
10. [Comprehensive Documentation](#10-comprehensive-documentation)

### Long Term
11. [Custom Metrics Beyond Time](#11-custom-metrics-beyond-time)
12. [Distributed Tracing Integration](#12-distributed-tracing-integration)
13. [Branch/Commit Comparison Tools](#13-branchcommit-comparison-tools)
14. [Plugin Marketplace for Extensions](#14-plugin-marketplace-for-extensions)
15. [Real-Time Performance Monitoring Dashboard](#15-real-time-performance-monitoring-dashboard)

---

## Immediate Actions

### 1. Baseline Comparison Feature
**Fidelity: 🟢 High**

#### Configuration Example
```typescript
const options: PerformanceOptions = {
  performanceResultsFileName: "performance-results",
  baselineComparison: {
    enabled: true,
    baselineFile: "performance-baseline.json",
    regressionThreshold: 10, // 10% increase triggers warning
    failOnRegression: true,
    autoUpdateBaseline: false,
    baselineStrategy: "median" // 'average' | 'best' | 'median'
  }
};
```

#### Console Output Example
```
Playwright-performance results(worker[0]):

╔════════════════════════════════════════════════════════════════════════════╗
║                     BASELINE COMPARISON REPORT                              ║
╚════════════════════════════════════════════════════════════════════════════╝

┌─────────────────┬──────────┬──────────┬──────────┬─────────┬──────────────┐
│ Sample Name     │ Current  │ Baseline │ Change   │ Status  │ Severity     │
├─────────────────┼──────────┼──────────┼──────────┼─────────┼──────────────┤
│ startup_GH      │ 1250ms   │ 1000ms   │ +25.0%   │ 🔴 FAIL │ CRITICAL     │
│ startup_SF      │ 850ms    │ 900ms    │ -5.6%    │ ✅ PASS │ IMPROVED     │
│ api_search      │ 320ms    │ 300ms    │ +6.7%    │ ✅ PASS │ MINOR        │
│ checkout_flow   │ 2100ms   │ 1800ms   │ +16.7%   │ 🔴 FAIL │ MAJOR        │
└─────────────────┴──────────┴──────────┴──────────┴─────────┴──────────────┘

Performance Analysis:
  ✅ 2 samples improved
  ⚠️  1 sample within threshold
  🔴 2 samples exceeded regression threshold

REGRESSIONS DETECTED:
  • startup_GH: +250ms (+25.0%) - Exceeds 10% threshold
  • checkout_flow: +300ms (+16.7%) - Exceeds 10% threshold

❌ Tests failed due to performance regressions
```

#### JSON Output Example
```json
{
  "comparisonResults": [
    {
      "name": "startup_GH",
      "brName": "chromium",
      "current": {
        "avgTime": 1250,
        "sem": 45,
        "repeats": 10,
        "minValue": 1100,
        "maxValue": 1400
      },
      "baseline": {
        "avgTime": 1000,
        "sem": 30,
        "repeats": 50
      },
      "comparison": {
        "absoluteChange": 250,
        "percentChange": 25.0,
        "isRegression": true,
        "severity": "critical",
        "exceedsThreshold": true
      }
    },
    {
      "name": "startup_SF",
      "brName": "chromium",
      "current": {
        "avgTime": 850,
        "sem": 25,
        "repeats": 10,
        "minValue": 800,
        "maxValue": 920
      },
      "baseline": {
        "avgTime": 900,
        "sem": 35,
        "repeats": 50
      },
      "comparison": {
        "absoluteChange": -50,
        "percentChange": -5.6,
        "isRegression": false,
        "severity": "improved",
        "exceedsThreshold": false
      }
    }
  ],
  "summary": {
    "totalSamples": 4,
    "improved": 2,
    "stable": 0,
    "regressed": 2,
    "failed": true
  }
}
```

#### HTML Report Enhancement
```html
<!-- Additional section in performance-chart.html -->
<div class="comparison-summary">
  <h3>📊 Baseline Comparison</h3>
  <div class="metrics">
    <div class="metric improved">
      <span class="value">2</span>
      <span class="label">Improved</span>
    </div>
    <div class="metric regressed">
      <span class="value">2</span>
      <span class="label">Regressed</span>
    </div>
    <div class="metric stable">
      <span class="value">0</span>
      <span class="label">Stable</span>
    </div>
  </div>

  <div class="regression-details">
    <h4>⚠️ Regressions Detected</h4>
    <ul>
      <li class="critical">
        <strong>startup_GH</strong>: 1000ms → 1250ms (+25.0%)
        <span class="badge">CRITICAL</span>
      </li>
      <li class="major">
        <strong>checkout_flow</strong>: 1800ms → 2100ms (+16.7%)
        <span class="badge">MAJOR</span>
      </li>
    </ul>
  </div>
</div>
```

---

### 2. Median and Percentile Statistics
**Fidelity: 🟢 High**

#### Console Output Example
```
Playwright-performance results(worker[0]):

┌──────────────┬──────────┬──────┬────────┬────────┬────────┬─────────┬─────┬─────────┬─────────┐
│ Sample       │ Browser  │ Avg  │ Median │ P95    │ P99    │ StdDev  │ SEM │ Min     │ Max     │
├──────────────┼──────────┼──────┼────────┼────────┼────────┼─────────┼─────┼─────────┼─────────┤
│ startup_GH   │ chromium │ 1250 │ 1200   │ 1450   │ 1580   │ 125     │ 45  │ 1100    │ 1600    │
│ startup_SF   │ chromium │ 850  │ 840    │ 920    │ 980    │ 75      │ 25  │ 800     │ 1000    │
│ api_search   │ chromium │ 320  │ 315    │ 380    │ 420    │ 40      │ 15  │ 280     │ 450     │
└──────────────┴──────────┴──────┴────────┴────────┴────────┴─────────┴─────┴─────────┴─────────┘

Samples: 10 per metric
Time unit: milliseconds

Performance Insights:
  • startup_GH: High variability (CV: 10.0%) - Consider investigating outliers
  • startup_SF: Stable performance (CV: 8.8%)
  • api_search: Very stable (CV: 12.5%)

Note: Median is more robust to outliers. P95/P99 represent worst-case scenarios.
```

#### JSON Output Example
```json
[
  {
    "name": "startup_GH",
    "brName": "chromium",
    "repeats": 10,
    "statistics": {
      "avgTime": 1250,
      "median": 1200,
      "p95": 1450,
      "p99": 1580,
      "stdDev": 125,
      "sem": 45,
      "minValue": 1100,
      "maxValue": 1600,
      "cv": 10.0,
      "outlierCount": 1
    },
    "interpretation": {
      "stability": "moderate",
      "recommendation": "Consider investigating outliers"
    }
  }
]
```

#### CSV Output Example
```csv
name,brName,avgTime,median,p95,p99,stdDev,sem,repeats,minValue,maxValue,cv,outlierCount
startup_GH,chromium,1250,1200,1450,1580,125,45,10,1100,1600,10.0,1
startup_SF,chromium,850,840,920,980,75,25,10,800,1000,8.8,0
api_search,chromium,320,315,380,420,40,15,10,280,450,12.5,1
```

---

### 3. Improved Error Messages and Validation
**Fidelity: 🟢 High**

#### Error Examples

**Missing sampleEnd:**
```
❌ Error: Unmatched performance sample detected

  Sample Name: "checkout_flow"
  Test: "e2e-checkout.spec.ts:25"
  Issue: sampleStart() called but sampleEnd() was never called

  This usually happens when:
    • Test failed before reaching sampleEnd()
    • Async operation not awaited
    • Early return in test logic

  Fix:
    • Ensure sampleEnd() is called in try/finally block
    • Check for conditional returns before sampleEnd()
    • Use dropResultsFromFailedTest: true to ignore failed test samples

  Code example:
    try {
      performance.sampleStart("checkout_flow");
      // ... your test code
    } finally {
      performance.sampleEnd("checkout_flow");
    }
```

**Duplicate sample name in same test:**
```
⚠️  Warning: Duplicate sample name detected

  Sample Name: "page_load"
  Test: "navigation.spec.ts:15"
  Instance: 2nd occurrence in same test

  Previous usage: line 12 in same test

  Recommendation:
    • Use unique names like "page_load_home", "page_load_about"
    • Or clear samples between measurements

  Impact: Only the last measurement will be recorded
```

**Invalid sample name:**
```
❌ Error: Invalid sample name

  Sample Name: "user@login#flow!"
  Invalid characters: @, #, !

  Sample names must:
    • Be 3-100 characters long
    • Contain only: letters, numbers, hyphens, underscores
    • Not start with a number

  Valid examples:
    ✅ "user_login_flow"
    ✅ "api-response-time"
    ✅ "page_load_2"
    ❌ "2_page_load" (starts with number)
    ❌ "user@login" (contains @)
```

**Configuration validation:**
```
❌ Configuration Error: Invalid performance options

  Option: regressionThreshold
  Value: -5
  Issue: Must be a positive number between 0 and 100

  Current config:
    {
      regressionThreshold: -5  // ❌ Invalid
    }

  Correct config:
    {
      regressionThreshold: 10  // ✅ 10% threshold
    }
```

**File system errors:**
```
❌ Error: Cannot write performance results

  Path: /read-only-dir/performance-results/
  Issue: Permission denied (EACCES)

  Possible solutions:
    1. Check directory permissions: chmod 755 /read-only-dir/
    2. Change output directory in config:
       performanceResultsDirectoryName: "./my-results"
    3. Run with appropriate permissions

  Need help? https://github.com/tzurp/playwright-performance/issues
```

---

### 4. Performance Budget Enforcement
**Fidelity: 🟢 High**

#### Configuration Example
```typescript
const options: PerformanceOptions = {
  performanceBudgets: [
    {
      sampleName: "startup_GH",
      maxAvgTime: 1000,        // Fail if average > 1000ms
      maxP95Time: 1500,        // Fail if P95 > 1500ms
      maxRegressionPercent: 10 // Fail if regression > 10%
    },
    {
      sampleName: "checkout_flow",
      maxAvgTime: 2000,
      maxP99Time: 3000
    },
    {
      sampleName: "api_*",      // Wildcard pattern
      maxAvgTime: 500
    }
  ],
  budgetFailureStrategy: "fail-fast" // or "collect-all"
};
```

#### Console Output - Budget Violations
```
Playwright-performance Budget Report(worker[0]):

╔════════════════════════════════════════════════════════════════════════════╗
║                     PERFORMANCE BUDGET VIOLATIONS                           ║
╚════════════════════════════════════════════════════════════════════════════╝

┌─────────────────┬────────────┬──────────┬──────────┬──────────┬──────────┐
│ Sample Name     │ Metric     │ Budget   │ Actual   │ Exceeded │ Status   │
├─────────────────┼────────────┼──────────┼──────────┼──────────┼──────────┤
│ startup_GH      │ Avg Time   │ 1000ms   │ 1250ms   │ +250ms   │ ❌ FAIL  │
│ startup_GH      │ P95 Time   │ 1500ms   │ 1450ms   │ -50ms    │ ✅ PASS  │
│ checkout_flow   │ Avg Time   │ 2000ms   │ 1850ms   │ -150ms   │ ✅ PASS  │
│ checkout_flow   │ P99 Time   │ 3000ms   │ 3200ms   │ +200ms   │ ❌ FAIL  │
│ api_search      │ Avg Time   │ 500ms    │ 320ms    │ -180ms   │ ✅ PASS  │
└─────────────────┴────────────┴──────────┴──────────┴──────────┴──────────┘

Budget Summary:
  ✅ 3 metrics within budget
  ❌ 2 metrics exceeded budget

BUDGET VIOLATIONS:
  1. startup_GH (Avg Time)
     Budget: 1000ms | Actual: 1250ms | Over by: 250ms (25%)

  2. checkout_flow (P99 Time)
     Budget: 3000ms | Actual: 3200ms | Over by: 200ms (6.7%)

❌ Performance budgets exceeded - Build should fail

Performance budgets help maintain performance SLAs. Consider:
  • Optimizing code paths for failed metrics
  • Adjusting budgets if targets are unrealistic
  • Investigating recent changes that may have impacted performance
```

#### JUnit XML Output (for CI)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<testsuites name="Performance Budgets" tests="5" failures="2" time="0.5">
  <testsuite name="Performance Budget: startup_GH" tests="2" failures="1">
    <testcase name="Avg Time <= 1000ms" time="0.1">
      <failure message="Budget exceeded">
        Expected: <= 1000ms
        Actual: 1250ms
        Over by: 250ms (25%)
      </failure>
    </testcase>
    <testcase name="P95 Time <= 1500ms" time="0.1">
      <!-- Passed -->
    </testcase>
  </testsuite>
  <testsuite name="Performance Budget: checkout_flow" tests="2" failures="1">
    <testcase name="Avg Time <= 2000ms" time="0.1">
      <!-- Passed -->
    </testcase>
    <testcase name="P99 Time <= 3000ms" time="0.1">
      <failure message="Budget exceeded">
        Expected: <= 3000ms
        Actual: 3200ms
        Over by: 200ms (6.7%)
      </failure>
    </testcase>
  </testsuite>
</testsuites>
```

---

### 5. Unit Tests for Core Components
**Fidelity: 🟢 High**

#### Test Structure Example
```typescript
// tests/unit/calculator.spec.ts
import { describe, it, expect } from '@playwright/test';
import calculator from '../../src/helpers/calculator';

describe('Calculator', () => {
  describe('getAverageAndStandardDeviation', () => {
    it('should calculate correct average and SEM for simple dataset', () => {
      const data = [100, 200, 300, 400, 500];
      const [avg, sem] = calculator.getAverageAndStandardDeviation(data);

      expect(avg).toBe(300);
      expect(sem).toBe(71); // ±71ms
    });

    it('should handle single value dataset', () => {
      const data = [100];
      const [avg, sem] = calculator.getAverageAndStandardDeviation(data);

      expect(avg).toBe(100);
      expect(sem).toBe(0); // No variation
    });

    it('should handle outliers correctly', () => {
      const data = [100, 105, 102, 98, 1000]; // One outlier
      const [avg, sem] = calculator.getAverageAndStandardDeviation(data);

      expect(avg).toBeGreaterThan(100);
      expect(sem).toBeGreaterThan(150); // High SEM indicates variability
    });
  });

  describe('getPercentile', () => {
    it('should calculate P95 correctly', () => {
      const data = Array.from({ length: 100 }, (_, i) => i + 1); // 1-100
      const p95 = calculator.getPercentile(data, 95);

      expect(p95).toBe(95);
    });

    it('should calculate median (P50) correctly', () => {
      const data = [1, 2, 3, 4, 5];
      const median = calculator.getPercentile(data, 50);

      expect(median).toBe(3);
    });
  });
});
```

#### Test Output Example
```
 PASS  tests/unit/calculator.spec.ts
  Calculator
    getAverageAndStandardDeviation
      ✓ should calculate correct average and SEM for simple dataset (3ms)
      ✓ should handle single value dataset (1ms)
      ✓ should handle outliers correctly (2ms)
    getPercentile
      ✓ should calculate P95 correctly (2ms)
      ✓ should calculate median (P50) correctly (1ms)

 PASS  tests/unit/baseline-comparator.spec.ts
  BaselineComparator
    compare
      ✓ should detect regression when current > baseline + threshold (5ms)
      ✓ should detect improvement when current < baseline (3ms)
      ✓ should mark as stable when within threshold (2ms)
    calculateSeverity
      ✓ should return critical for >20% regression (1ms)
      ✓ should return major for 10-20% regression (1ms)
      ✓ should return minor for <10% regression (1ms)

 PASS  tests/unit/performance-cache.spec.ts
  PerformanceCache
    ✓ should match sampleStart with sampleEnd correctly (4ms)
    ✓ should handle multiple concurrent samples (6ms)
    ✓ should calculate duration correctly (2ms)
    ✓ should warn on unmatched samples (3ms)

Test Suites: 3 passed, 3 total
Tests:       14 passed, 14 total
Snapshots:   0 total
Time:        2.456s
Coverage:    87.3% Statements, 82.1% Branches, 91.2% Functions, 88.5% Lines
```

---

## Short Term Features

### 6. Trend Analysis and Historical Reporting
**Fidelity: 🟡 Medium**

#### Console Output Example
```
Playwright-performance Trend Analysis(Last 30 days):

╔════════════════════════════════════════════════════════════════════════════╗
║                          PERFORMANCE TRENDS                                 ║
╚════════════════════════════════════════════════════════════════════════════╝

┌─────────────────┬───────────┬──────────────┬────────────┬──────────────────┐
│ Sample Name     │ Current   │ 7d Ago       │ 30d Ago    │ Trend            │
├─────────────────┼───────────┼──────────────┼────────────┼──────────────────┤
│ startup_GH      │ 1250ms    │ 1100ms       │ 950ms      │ 📈 +31.6% (bad)  │
│ startup_SF      │ 850ms     │ 880ms        │ 920ms      │ 📉 -7.6% (good)  │
│ api_search      │ 320ms     │ 310ms        │ 305ms      │ → +4.9% (stable) │
│ checkout_flow   │ 2100ms    │ 2050ms       │ 2000ms     │ 📈 +5.0% (watch) │
└─────────────────┴───────────┴──────────────┴────────────┴──────────────────┘

Detailed Trend Analysis:

  📈 startup_GH (DEGRADING)
     ├─ Average change: +10ms/day
     ├─ Total change: +300ms over 30 days
     ├─ Acceleration: Increasing (trend is worsening)
     ├─ Prediction: Will reach 1400ms in 15 days if trend continues
     └─ ⚠️  ACTION REQUIRED: Investigate recent changes

  📉 startup_SF (IMPROVING)
     ├─ Average change: -2.3ms/day
     ├─ Total change: -70ms over 30 days
     ├─ ✅ Good: Consistent improvement

  → api_search (STABLE)
     ├─ Variation: ±15ms
     ├─ ✅ Stable performance

  📊 Data Points: 156 test runs across 30 days
```

#### JSON Trend Data
```json
{
  "trendAnalysis": {
    "periodDays": 30,
    "totalRuns": 156,
    "samples": [
      {
        "name": "startup_GH",
        "trend": "degrading",
        "current": 1250,
        "sevenDaysAgo": 1100,
        "thirtyDaysAgo": 950,
        "changeRate": 10.0,
        "totalChange": 300,
        "percentChange": 31.6,
        "acceleration": "increasing",
        "prediction": {
          "value": 1400,
          "daysToReach": 15,
          "confidence": 0.85
        },
        "dataPoints": [
          { "date": "2026-01-15", "avgTime": 950 },
          { "date": "2026-01-16", "avgTime": 962 },
          { "date": "2026-01-17", "avgTime": 955 },
          // ... more data points
          { "date": "2026-02-14", "avgTime": 1250 }
        ],
        "actionRequired": true,
        "severity": "high"
      }
    ]
  }
}
```

#### HTML Trend Chart
```html
<!-- Time-series line chart showing trends -->
<div class="trend-charts">
  <h3>📈 Performance Trends (30 Days)</h3>

  <canvas id="trendChart"></canvas>

  <script>
    // Chart.js configuration for time-series
    new Chart(ctx, {
      type: 'line',
      data: {
        datasets: [
          {
            label: 'startup_GH',
            data: [
              { x: '2026-01-15', y: 950 },
              { x: '2026-01-16', y: 962 },
              // ... more points
              { x: '2026-02-14', y: 1250 }
            ],
            borderColor: 'rgb(255, 99, 132)',
            backgroundColor: 'rgba(255, 99, 132, 0.1)',
            tension: 0.4
          },
          {
            label: 'startup_SF',
            data: [...],
            borderColor: 'rgb(75, 192, 192)',
            backgroundColor: 'rgba(75, 192, 192, 0.1)',
            tension: 0.4
          }
        ]
      },
      options: {
        scales: {
          x: { type: 'time', time: { unit: 'day' } },
          y: { beginAtZero: true, title: { display: true, text: 'Time (ms)' } }
        },
        plugins: {
          annotation: {
            annotations: {
              threshold: {
                type: 'line',
                yMin: 1000,
                yMax: 1000,
                borderColor: 'rgb(255, 99, 132)',
                borderWidth: 2,
                borderDash: [5, 5],
                label: { content: 'Threshold', enabled: true }
              }
            }
          }
        }
      }
    });
  </script>
</div>
```

---

### 7. Enhanced HTML Charts with Interactivity
**Fidelity: 🟡 Medium**

#### Interactive HTML Report Example
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Performance Results - Interactive Dashboard</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-datalabels"></script>
  <style>
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
      margin: 20px;
      background: #f5f5f5;
    }
    .dashboard {
      display: grid;
      grid-template-columns: 300px 1fr;
      gap: 20px;
    }
    .sidebar {
      background: white;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    .filters { margin-bottom: 20px; }
    .filter-group { margin-bottom: 15px; }
    .filter-group label { display: block; margin-bottom: 5px; font-weight: 600; }
    .charts-area {
      background: white;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    .tabs {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
      border-bottom: 2px solid #e0e0e0;
    }
    .tab {
      padding: 10px 20px;
      cursor: pointer;
      border: none;
      background: none;
      font-size: 16px;
    }
    .tab.active {
      border-bottom: 3px solid #1976d2;
      color: #1976d2;
      font-weight: 600;
    }
    .export-buttons {
      margin-top: 20px;
      display: flex;
      gap: 10px;
    }
    .btn {
      padding: 8px 16px;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-size: 14px;
    }
    .btn-primary { background: #1976d2; color: white; }
    .btn-secondary { background: #666; color: white; }
    .comparison-mode {
      background: #fff3cd;
      padding: 15px;
      border-radius: 4px;
      margin-bottom: 20px;
    }
  </style>
</head>
<body>
  <h1>🎭 Playwright Performance Dashboard</h1>
  <p>Generated: 2026-02-14 15:30:45 | Test runs: 156 | Period: Last 30 days</p>

  <div class="dashboard">
    <!-- Sidebar with filters -->
    <div class="sidebar">
      <h3>Filters</h3>
      <div class="filters">
        <div class="filter-group">
          <label>Browser</label>
          <select id="browserFilter" onchange="applyFilters()">
            <option value="all">All Browsers</option>
            <option value="chromium">Chromium</option>
            <option value="firefox">Firefox</option>
            <option value="webkit">WebKit</option>
          </select>
        </div>

        <div class="filter-group">
          <label>Date Range</label>
          <select id="dateRange" onchange="applyFilters()">
            <option value="7">Last 7 days</option>
            <option value="30" selected>Last 30 days</option>
            <option value="90">Last 90 days</option>
            <option value="all">All time</option>
          </select>
        </div>

        <div class="filter-group">
          <label>Sort By</label>
          <select id="sortBy" onchange="applyFilters()">
            <option value="avgTime">Average Time</option>
            <option value="name">Name</option>
            <option value="trend">Trend</option>
            <option value="variance">Variability</option>
          </select>
        </div>

        <div class="filter-group">
          <label>
            <input type="checkbox" id="showOutliers" onchange="applyFilters()">
            Show outliers
          </label>
        </div>
      </div>

      <hr>

      <h3>Compare Mode</h3>
      <div class="filter-group">
        <label>
          <input type="checkbox" id="compareMode" onchange="toggleCompareMode()">
          Enable comparison
        </label>
      </div>
      <div id="compareOptions" style="display: none;">
        <select id="compareBaseline">
          <option value="baseline">vs Baseline</option>
          <option value="7days">vs 7 days ago</option>
          <option value="30days">vs 30 days ago</option>
          <option value="commit">vs Commit...</option>
        </select>
      </div>

      <hr>

      <div class="export-buttons">
        <button class="btn btn-primary" onclick="exportPNG()">📷 Export PNG</button>
        <button class="btn btn-secondary" onclick="exportCSV()">📊 Export CSV</button>
        <button class="btn btn-secondary" onclick="exportPDF()">📄 Export PDF</button>
      </div>
    </div>

    <!-- Main chart area with tabs -->
    <div class="charts-area">
      <div class="tabs">
        <button class="tab active" onclick="showTab('overview')">Overview</button>
        <button class="tab" onclick="showTab('trends')">Trends</button>
        <button class="tab" onclick="showTab('comparison')">Comparison</button>
        <button class="tab" onclick="showTab('distribution')">Distribution</button>
      </div>

      <div id="overviewTab" class="tab-content">
        <h2>Performance Overview</h2>
        <canvas id="overviewChart" width="800" height="400"></canvas>
      </div>

      <div id="trendsTab" class="tab-content" style="display:none;">
        <h2>Performance Trends</h2>
        <canvas id="trendsChart" width="800" height="400"></canvas>
      </div>

      <div id="comparisonTab" class="tab-content" style="display:none;">
        <div class="comparison-mode">
          ℹ️ Comparing current results with baseline from 2026-01-15
        </div>
        <canvas id="comparisonChart" width="800" height="400"></canvas>
      </div>

      <div id="distributionTab" class="tab-content" style="display:none;">
        <h2>Performance Distribution (Box Plot)</h2>
        <canvas id="distributionChart" width="800" height="400"></canvas>
      </div>
    </div>
  </div>

  <script>
    // Interactive functionality
    function applyFilters() {
      const browser = document.getElementById('browserFilter').value;
      const dateRange = document.getElementById('dateRange').value;
      const sortBy = document.getElementById('sortBy').value;

      // Update charts based on filters
      console.log('Applying filters:', { browser, dateRange, sortBy });
      updateCharts();
    }

    function toggleCompareMode() {
      const enabled = document.getElementById('compareMode').checked;
      document.getElementById('compareOptions').style.display =
        enabled ? 'block' : 'none';

      if (enabled) {
        showTab('comparison');
      }
    }

    function showTab(tabName) {
      // Hide all tabs
      document.querySelectorAll('.tab-content').forEach(t => t.style.display = 'none');
      document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));

      // Show selected tab
      document.getElementById(tabName + 'Tab').style.display = 'block';
      event.target.classList.add('active');
    }

    function exportPNG() {
      const canvas = document.querySelector('canvas:not([style*="display: none"])');
      const url = canvas.toDataURL('image/png');
      const a = document.createElement('a');
      a.href = url;
      a.download = 'performance-chart-' + new Date().toISOString() + '.png';
      a.click();
    }

    function exportCSV() {
      // Export current filtered data as CSV
      window.location.href = 'performance-results.csv';
    }

    function exportPDF() {
      window.print();
    }
  </script>
</body>
</html>
```

**Features Shown:**
- ✅ Multi-tab interface (Overview, Trends, Comparison, Distribution)
- ✅ Interactive filters (Browser, Date Range, Sort)
- ✅ Comparison mode toggle
- ✅ Export functionality (PNG, CSV, PDF)
- ✅ Responsive layout
- ✅ Real-time chart updates

---

### 8. CI/CD Integration Helpers
**Fidelity: 🟢 High**

#### GitHub Actions Summary Output
```markdown
## 🎭 Playwright Performance Report

**Test Run:** #1234 | **Branch:** feature/checkout-optimization | **Commit:** a1b2c3d

### 📊 Performance Summary

| Metric | Value |
|--------|-------|
| Total Samples | 4 |
| ✅ Within Budget | 2 |
| ⚠️ Near Threshold | 1 |
| ❌ Budget Exceeded | 1 |

### 📈 Performance Results

| Sample | Browser | Current | Baseline | Change | Status |
|--------|---------|---------|----------|--------|--------|
| startup_GH | chromium | 1250ms | 1000ms | +25.0% 📈 | ❌ FAIL |
| startup_SF | chromium | 850ms | 900ms | -5.6% 📉 | ✅ PASS |
| api_search | chromium | 320ms | 300ms | +6.7% → | ⚠️ WARN |
| checkout_flow | chromium | 2100ms | 1800ms | +16.7% 📈 | ❌ FAIL |

### ⚠️ Regressions Detected

- **startup_GH**: 1000ms → 1250ms (+250ms, +25.0%)
- **checkout_flow**: 1800ms → 2100ms (+300ms, +16.7%)

### 💡 Recommendations

- Investigate recent changes in startup_GH and checkout_flow
- Consider reverting commit a1b2c3d if regressions are unacceptable
- Review performance budgets if targets are unrealistic

---
📊 [View Detailed Report](https://example.com/performance-results/run-1234.html) | 📈 [Trend Analysis](https://example.com/performance-trends)
```

#### GitLab CI Performance Widget Data
```json
{
  "metrics": [
    {
      "name": "startup_GH",
      "value": 1250,
      "unit": "ms",
      "desiredSize": "smaller",
      "previous": 1000
    },
    {
      "name": "startup_SF",
      "value": 850,
      "unit": "ms",
      "desiredSize": "smaller",
      "previous": 900
    },
    {
      "name": "api_search",
      "value": 320,
      "unit": "ms",
      "desiredSize": "smaller",
      "previous": 300
    },
    {
      "name": "checkout_flow",
      "value": 2100,
      "unit": "ms",
      "desiredSize": "smaller",
      "previous": 1800
    }
  ]
}
```

#### Slack Notification
```
🎭 *Playwright Performance Alert*

*Build:* #1234 | *Branch:* `feature/checkout-optimization`
*Status:* ❌ Performance Regression Detected

*Regressions:*
• `startup_GH`: 1000ms → 1250ms (+25.0%)
• `checkout_flow`: 1800ms → 2100ms (+16.7%)

*Improvements:*
• `startup_SF`: 900ms → 850ms (-5.6%)

<https://github.com/user/repo/actions/runs/1234|View Details> | <https://example.com/performance-trends|Trends>
```

#### JUnit XML Format (Already shown in #4)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<testsuites name="Performance Tests" tests="4" failures="2" time="5.42">
  <testsuite name="Performance Metrics" tests="4" failures="2">
    <testcase name="startup_GH performance" time="1.25">
      <failure message="Performance regression: +25.0%">
        Baseline: 1000ms
        Current: 1250ms
        Change: +250ms (+25.0%)
        Threshold: 10%
      </failure>
    </testcase>
    <testcase name="startup_SF performance" time="0.85"/>
    <testcase name="api_search performance" time="0.32"/>
    <testcase name="checkout_flow performance" time="2.1">
      <failure message="Performance regression: +16.7%">
        Baseline: 1800ms
        Current: 2100ms
        Change: +300ms (+16.7%)
        Threshold: 10%
      </failure>
    </testcase>
  </testsuite>
</testsuites>
```

#### Configuration Example
```typescript
const options: PerformanceOptions = {
  ciIntegration: {
    enabled: true,
    providers: ['github', 'gitlab', 'jenkins'],

    github: {
      outputSummary: true,      // Job summary markdown
      createAnnotations: true,   // PR annotations
      failOnRegression: true
    },

    gitlab: {
      performanceMetrics: true,  // Performance widget
      metricsFile: 'performance.json'
    },

    notifications: {
      slack: {
        webhookUrl: process.env.SLACK_WEBHOOK,
        onlyOnFailure: true,
        mentionUsers: ['@perf-team']
      },
      email: {
        recipients: ['team@example.com'],
        onlyOnFailure: true
      }
    },

    artifacts: {
      uploadToS3: true,
      bucket: 'performance-reports',
      retention: 90  // days
    }
  }
};
```

---

### 9. Stream-Based Analysis for Scalability
**Fidelity: 🟡 Medium**

#### Console Output Example
```
Playwright-performance Stream Analysis:

📊 Processing performance data...
  ├─ Reading log file: performance-log.json
  ├─ File size: 247 MB
  ├─ Estimated entries: ~1,000,000
  └─ Using stream-based processing

Progress: [████████████████████████████] 100%

✅ Processed 1,000,000 entries in 12.3 seconds
  ├─ Memory used: 45 MB (peak)
  ├─ Processing rate: 81,300 entries/sec
  └─ Traditional approach would use: ~2.1 GB

Performance Results:
  ├─ Unique samples: 25
  ├─ Total duration: 156.2 hours
  └─ Date range: 2025-01-15 to 2026-02-14

Results saved to: performance-results.json/csv
```

#### Technical Implementation Output
```typescript
// Stream processing logs
[2026-02-14 15:30:01] Starting stream-based analysis
[2026-02-14 15:30:01] Opening read stream: performance-log.json
[2026-02-14 15:30:01] Chunk size: 64 KB
[2026-02-14 15:30:01] Parallel workers: 4

[2026-02-14 15:30:02] Processed 100,000 entries (Memory: 12 MB)
[2026-02-14 15:30:04] Processed 250,000 entries (Memory: 18 MB)
[2026-02-14 15:30:06] Processed 500,000 entries (Memory: 28 MB)
[2026-02-14 15:30:10] Processed 750,000 entries (Memory: 35 MB)
[2026-02-14 15:30:13] Processed 1,000,000 entries (Memory: 45 MB)

[2026-02-14 15:30:13] Finalizing statistics...
[2026-02-14 15:30:14] Writing results...
[2026-02-14 15:30:14] ✅ Complete

Performance Comparison:
┌─────────────────────┬─────────────┬──────────────┬───────────┐
│ Method              │ Time        │ Peak Memory  │ Scalable  │
├─────────────────────┼─────────────┼──────────────┼───────────┤
│ Load All (current)  │ 45.2s       │ 2.1 GB       │ ❌ No     │
│ Stream-based (new)  │ 12.3s       │ 45 MB        │ ✅ Yes    │
│ Improvement         │ 3.7x faster │ 46x less     │ ∞ scale   │
└─────────────────────┴─────────────┴──────────────┴───────────┘
```

#### Configuration
```typescript
const options: PerformanceOptions = {
  streamProcessing: {
    enabled: true,              // Auto-enable for files > 100MB
    chunkSize: 65536,           // 64 KB chunks
    parallelWorkers: 4,         // Use worker threads
    progressUpdates: true,      // Show progress
    memoryLimit: 512 * 1024 * 1024  // 512 MB limit
  }
};
```

---

### 10. Comprehensive Documentation
**Fidelity: 🟡 Medium**

#### API Documentation Example
```markdown
# Playwright-Performance API Reference

## Configuration Options

### `PerformanceOptions`

Complete configuration object for playwright-performance plugin.

```typescript
interface PerformanceOptions {
  // Basic Options
  disableAppendToExistingFile?: boolean;
  dropResultsFromFailedTest?: boolean;
  performanceResultsDirectoryName?: string;
  performanceResultsFileName?: string;

  // Analysis Options
  analyzeByBrowser?: boolean;
  recentDays?: number;
  suppressConsoleResults?: boolean;

  // Visualization
  generateHtmlChart?: boolean;

  // NEW: Baseline Comparison
  baselineComparison?: {
    enabled: boolean;
    baselineFile: string;
    regressionThreshold: number;
    failOnRegression?: boolean;
    autoUpdateBaseline?: boolean;
    baselineStrategy?: 'average' | 'best' | 'median';
  };

  // NEW: Performance Budgets
  performanceBudgets?: Array<{
    sampleName: string;
    maxAvgTime?: number;
    maxP95Time?: number;
    maxP99Time?: number;
    maxRegressionPercent?: number;
  }>;

  // NEW: CI/CD Integration
  ciIntegration?: {
    enabled: boolean;
    providers?: string[];
    github?: GitHubOptions;
    gitlab?: GitLabOptions;
    notifications?: NotificationOptions;
  };
}
```

#### Examples

##### Example 1: Basic Usage
```typescript
import { test as base } from '@playwright/test';
import extendPlaywrightPerformance from 'playwright-performance';

const test = base.extend(extendPlaywrightPerformance());

test('measure page load', async ({ page, performance }) => {
  performance.sampleStart('page_load');
  await page.goto('https://example.com');
  performance.sampleEnd('page_load');

  const duration = performance.getSampleTime('page_load');
  expect(duration).toBeLessThan(1000);
});
```

##### Example 2: With Baseline Comparison
```typescript
const options = {
  baselineComparison: {
    enabled: true,
    baselineFile: 'performance-baseline.json',
    regressionThreshold: 10,
    failOnRegression: true
  }
};

const test = base.extend(extendPlaywrightPerformance(options));
```

##### Example 3: With Performance Budgets
```typescript
const options = {
  performanceBudgets: [
    {
      sampleName: 'checkout_*',
      maxAvgTime: 2000,
      maxP95Time: 3000
    }
  ]
};
```
```

#### Troubleshooting Guide
```markdown
# Troubleshooting Guide

## Common Issues

### Issue: "Unmatched performance sample"

**Symptom:**
```
Error: Unmatched performance sample detected
Sample Name: "checkout_flow"
```

**Cause:** `sampleEnd()` was not called after `sampleStart()`

**Solution:**
```typescript
// ❌ Wrong
performance.sampleStart('test');
await page.goto('...');
if (condition) return;  // Early return!
performance.sampleEnd('test');

// ✅ Correct
try {
  performance.sampleStart('test');
  await page.goto('...');
  if (condition) return;
} finally {
  performance.sampleEnd('test');
}
```

### Issue: Performance results file is huge

**Symptom:** `performance-log.json` is several GB

**Cause:** Long-running test suite with `disableAppendToExistingFile: false`

**Solutions:**
1. Enable data retention:
```typescript
{
  dataRetention: {
    maxLogSizeMB: 100,
    archiveOlderThanDays: 30
  }
}
```

2. Use `recentDays` to analyze only recent data:
```typescript
{
  recentDays: 7  // Only analyze last 7 days
}
```

3. Periodically clear old data manually
```

---

## Long Term Features

### 11. Custom Metrics Beyond Time
**Fidelity: 🟡 Medium**

#### Usage Example
```typescript
test('measure with custom metrics', async ({ page, performance }) => {
  performance.sampleStart('api_call');

  const response = await page.request.get('https://api.example.com/data');
  const data = await response.json();

  // Add custom metrics
  performance.addMetric('api_call', 'responseSize', data.length);
  performance.addMetric('api_call', 'statusCode', response.status());
  performance.addMetric('api_call', 'itemCount', data.items.length);

  // Track memory usage
  const memory = await page.evaluate(() =>
    performance.memory?.usedJSHeapSize || 0
  );
  performance.addMetric('api_call', 'memoryUsed', memory);

  performance.sampleEnd('api_call');
});
```

#### Console Output
```
Playwright-performance results(worker[0]):

┌──────────────┬──────────┬──────────┬──────────────┬─────────────┬────────────┐
│ Sample       │ Duration │ Response │ Memory Used  │ Item Count  │ Status     │
│              │ (ms)     │ Size(KB) │ (MB)         │             │ Code       │
├──────────────┼──────────┼──────────┼──────────────┼─────────────┼────────────┤
│ api_call     │ 320      │ 245      │ 12.5         │ 150         │ 200        │
│ api_search   │ 180      │ 89       │ 8.2          │ 45          │ 200        │
│ api_filter   │ 95       │ 12       │ 6.1          │ 8           │ 200        │
└──────────────┴──────────┴──────────┴──────────────┴─────────────┴────────────┘

Custom Metrics Summary:
  • Total response size: 346 KB
  • Avg memory footprint: 8.9 MB
  • Total items processed: 203
```

#### JSON Output
```json
{
  "name": "api_call",
  "brName": "chromium",
  "timeMetrics": {
    "avgTime": 320,
    "median": 315,
    "p95": 380,
    "sem": 15
  },
  "customMetrics": {
    "responseSize": {
      "values": [245120, 243890, 246780],
      "avg": 245263,
      "unit": "bytes"
    },
    "memoryUsed": {
      "values": [13107200, 12582912, 12845056],
      "avg": 12845056,
      "unit": "bytes"
    },
    "itemCount": {
      "values": [150, 148, 152],
      "avg": 150,
      "unit": "count"
    },
    "statusCode": {
      "values": [200, 200, 200],
      "mode": 200,
      "distribution": { "200": 3 }
    }
  }
}
```

---

### 12. Distributed Tracing Integration
**Fidelity: 🟠 Low**

#### Configuration Example
```typescript
const options: PerformanceOptions = {
  distributedTracing: {
    enabled: true,
    exporter: 'jaeger',  // or 'zipkin', 'otlp'
    serviceName: 'playwright-tests',
    endpoint: 'http://localhost:14268/api/traces',

    // Correlation
    propagateContext: true,
    headerFormat: 'w3c-traceparent'
  }
};
```

#### Visual Output - Jaeger UI
```
Trace View: checkout_flow_test

Span Timeline:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2100ms

playwright.test: checkout_flow_test
├─ playwright.sample: checkout_flow (2100ms)
│  ├─ browser.navigation: goto /checkout (450ms)
│  │  └─ backend.api: GET /checkout (380ms)
│  │     ├─ db.query: SELECT products (120ms)
│  │     ├─ db.query: SELECT user (45ms)
│  │     └─ cache.get: session (5ms)
│  ├─ browser.interaction: fill form (200ms)
│  ├─ browser.interaction: click submit (50ms)
│  └─ browser.navigation: goto /confirmation (1400ms)
│     └─ backend.api: POST /order (1350ms)
│        ├─ db.transaction: create order (800ms)
│        ├─ payment.service: process (450ms)
│        └─ email.service: send confirmation (100ms)

Tags:
  browser: chromium
  test.file: checkout.spec.ts
  test.line: 42
  environment: staging

Logs:
  15:30:01.000  Starting checkout flow
  15:30:01.450  Form filled successfully
  15:30:01.650  Submitting order
  15:30:03.000  Order confirmed: #12345
```

#### Benefits Visualization
```
Before (isolated):
  Playwright Test: 2100ms
  (No visibility into backend)

After (distributed tracing):
  Playwright Test: 2100ms
  ├─ Frontend: 700ms
  ├─ Backend API: 1350ms
  │  ├─ Database: 965ms ⚠️ SLOW
  │  └─ Payment: 450ms
  └─ Network: 50ms

👉 Insight: Database is the bottleneck, not frontend!
```

---

### 13. Branch/Commit Comparison Tools
**Fidelity: 🟡 Medium**

#### CLI Usage
```bash
# Compare current branch with main
$ npx playwright-perf compare --baseline origin/main --current HEAD

🎭 Playwright Performance Comparison

Comparing:
  📊 Baseline: origin/main (commit: abc123f)
  🆕 Current:  HEAD (commit: xyz789d)

Results:
┌─────────────────┬────────────┬────────────┬──────────┬─────────────────┐
│ Sample Name     │ Main       │ Current    │ Change   │ Significance    │
├─────────────────┼────────────┼────────────┼──────────┼─────────────────┤
│ startup_GH      │ 1000ms     │ 1250ms     │ +25.0%   │ 🔴 SIGNIFICANT  │
│ startup_SF      │ 900ms      │ 850ms      │ -5.6%    │ 🟢 IMPROVEMENT  │
│ api_search      │ 300ms      │ 320ms      │ +6.7%    │ 🟡 MINOR        │
│ checkout_flow   │ 1800ms     │ 2100ms     │ +16.7%   │ 🔴 SIGNIFICANT  │
└─────────────────┴────────────┴────────────┴──────────┴─────────────────┘

Statistical Analysis:
  • Performed t-test on differences
  • Confidence level: 95%
  • Significant changes marked with 🔴

Summary:
  ✅ 1 improvement
  ⚠️  1 minor regression
  ❌ 2 significant regressions

Recommendation: ❌ Do not merge - significant performance regressions detected
```

#### GitHub PR Comment
```markdown
## 🎭 Performance Comparison Report

Comparing `feature/checkout-optimization` against `main`

### Summary
- ✅ 1 improvement
- ⚠️ 1 minor change
- ❌ 2 significant regressions

### Detailed Results

| Sample | Main | This PR | Change | Verdict |
|--------|------|---------|--------|---------|
| startup_GH | 1000ms | 1250ms | +250ms (+25.0%) | ❌ Slower |
| startup_SF | 900ms | 850ms | -50ms (-5.6%) | ✅ Faster |
| api_search | 300ms | 320ms | +20ms (+6.7%) | ⚠️ Slightly slower |
| checkout_flow | 1800ms | 2100ms | +300ms (+16.7%) | ❌ Slower |

### Recommendations

❌ **This PR introduces significant performance regressions**

- `startup_GH` is 25% slower
- `checkout_flow` is 16.7% slower

Consider investigating the following commits:
- xyz789d: "Add new feature X" (+200ms impact estimated)
- def456g: "Refactor module Y" (+100ms impact estimated)

[View Full Report](https://example.com/perf/pr/123) | [Historical Trends](https://example.com/perf/trends)
```

#### Configuration
```typescript
const options: PerformanceOptions = {
  comparison: {
    enabled: true,
    autoCommentOnPR: true,
    failOnRegression: true,
    statisticalSignificance: 0.05,  // p-value threshold
    minimumSamples: 3  // Require at least 3 samples for valid comparison
  }
};
```

---

### 14. Plugin Marketplace for Extensions
**Fidelity: 🟠 Low**

#### Marketplace Interface (Conceptual)
```
╔════════════════════════════════════════════════════════════════════════╗
║               Playwright-Performance Plugin Marketplace                 ║
╚════════════════════════════════════════════════════════════════════════╝

Search: [_________________________] 🔍   Categories: [All ▾]

┌─────────────────────────────────────────────────────────────────────────┐
│ 📦 playwright-perf-webvitals                           ⭐⭐⭐⭐⭐ (245)  │
│                                                                          │
│ Automatically collect Core Web Vitals (LCP, FID, CLS) during tests     │
│                                                                          │
│ npm install playwright-perf-webvitals                                   │
│                                                                          │
│ Downloads: 12.5K/week │ Updated: 2 days ago │ License: MIT             │
└─────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────┐
│ 📦 playwright-perf-memory                              ⭐⭐⭐⭐ (189)    │
│                                                                          │
│ Track memory usage, heap snapshots, and detect memory leaks            │
│                                                                          │
│ npm install playwright-perf-memory                                      │
│                                                                          │
│ Downloads: 8.2K/week │ Updated: 1 week ago │ License: Apache-2.0       │
└─────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────┐
│ 📦 playwright-perf-lighthouse                          ⭐⭐⭐⭐⭐ (512)  │
│                                                                          │
│ Run Lighthouse audits and include scores in performance reports        │
│                                                                          │
│ npm install playwright-perf-lighthouse                                  │
│                                                                          │
│ Downloads: 25.3K/week │ Updated: 1 day ago │ License: MIT              │
└─────────────────────────────────────────────────────────────────────────┘
```

#### Plugin Usage Example
```typescript
import extendPlaywrightPerformance from 'playwright-performance';
import webVitalsPlugin from 'playwright-perf-webvitals';
import memoryPlugin from 'playwright-perf-memory';

const options = {
  plugins: [
    webVitalsPlugin({
      includeMetrics: ['LCP', 'FID', 'CLS', 'FCP', 'TTFB'],
      failOnThreshold: {
        LCP: 2500,  // ms
        CLS: 0.1    // score
      }
    }),
    memoryPlugin({
      captureHeapSnapshots: true,
      detectLeaks: true
    })
  ]
};

const test = base.extend(extendPlaywrightPerformance(options));
```

#### Plugin Output Example
```
Playwright-performance results with plugins(worker[0]):

Core Metrics:
┌──────────────┬──────────┬──────────┐
│ Sample       │ Duration │ Status   │
├──────────────┼──────────┼──────────┤
│ page_load    │ 1250ms   │ ✅ PASS  │
└──────────────┴──────────┴──────────┘

🌐 Web Vitals (playwright-perf-webvitals):
┌──────────────┬──────────┬────────────┬──────────┐
│ Metric       │ Value    │ Threshold  │ Status   │
├──────────────┼──────────┼────────────┼──────────┤
│ LCP          │ 1850ms   │ < 2500ms   │ ✅ GOOD  │
│ FID          │ 45ms     │ < 100ms    │ ✅ GOOD  │
│ CLS          │ 0.05     │ < 0.1      │ ✅ GOOD  │
│ FCP          │ 980ms    │ -          │ ℹ️  INFO │
│ TTFB         │ 320ms    │ -          │ ℹ️  INFO │
└──────────────┴──────────┴────────────┴──────────┘

🧠 Memory Analysis (playwright-perf-memory):
┌──────────────────┬──────────┬──────────────────┐
│ Metric           │ Value    │ Status           │
├──────────────────┼──────────┼──────────────────┤
│ Heap Used        │ 45.2 MB  │ ✅ Normal        │
│ Heap Growth      │ +2.1 MB  │ ✅ Acceptable    │
│ Memory Leaks     │ 0        │ ✅ None detected │
│ DOM Nodes        │ 1,250    │ ✅ Normal        │
└──────────────────┴──────────┴──────────────────┘
```

---

### 15. Real-Time Performance Monitoring Dashboard
**Fidelity: 🟠 Low**

#### Dashboard Interface (Conceptual)
```html
<!DOCTYPE html>
<html>
<head>
  <title>Performance Monitoring Dashboard</title>
  <style>
    .dashboard {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
      gap: 20px;
      padding: 20px;
    }
    .widget {
      background: white;
      border-radius: 8px;
      padding: 20px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }
    .metric-value {
      font-size: 48px;
      font-weight: bold;
      color: #1976d2;
    }
    .trend {
      font-size: 18px;
      margin-top: 10px;
    }
    .trend.up { color: #d32f2f; }
    .trend.down { color: #388e3c; }
    .live-badge {
      display: inline-block;
      background: #4caf50;
      color: white;
      padding: 4px 8px;
      border-radius: 12px;
      font-size: 12px;
      animation: pulse 2s infinite;
    }
    @keyframes pulse {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.6; }
    }
  </style>
</head>
<body>
  <header>
    <h1>🎭 Playwright Performance Dashboard <span class="live-badge">● LIVE</span></h1>
    <p>Last updated: <span id="lastUpdate">2026-02-14 15:30:45</span></p>
  </header>

  <div class="dashboard">
    <!-- Widget 1: Average Response Time -->
    <div class="widget">
      <h3>Average Response Time</h3>
      <div class="metric-value">1,250ms</div>
      <div class="trend up">📈 +15% from last hour</div>
      <canvas id="avgTimeSparkline" width="200" height="50"></canvas>
    </div>

    <!-- Widget 2: Tests Running -->
    <div class="widget">
      <h3>Active Tests</h3>
      <div class="metric-value">3</div>
      <div class="trend">Running now</div>
      <ul>
        <li>checkout.spec.ts (worker 0)</li>
        <li>search.spec.ts (worker 1)</li>
        <li>auth.spec.ts (worker 2)</li>
      </ul>
    </div>

    <!-- Widget 3: Budget Status -->
    <div class="widget">
      <h3>Budget Compliance</h3>
      <div class="metric-value">75%</div>
      <div class="trend">3 of 4 within budget</div>
      <div style="margin-top: 10px;">
        ✅ startup_SF<br>
        ✅ api_search<br>
        ❌ startup_GH<br>
        ✅ checkout_flow
      </div>
    </div>

    <!-- Widget 4: Trend Alert -->
    <div class="widget" style="border-left: 4px solid #ff9800;">
      <h3>⚠️ Performance Alert</h3>
      <div>
        <strong>startup_GH</strong> is degrading
      </div>
      <div class="trend up">+10ms/day over 7 days</div>
      <button>Investigate</button>
    </div>

    <!-- Widget 5: Recent Runs -->
    <div class="widget">
      <h3>Recent Test Runs</h3>
      <table style="width: 100%; font-size: 14px;">
        <tr>
          <td>15:30:45</td>
          <td>checkout</td>
          <td>✅ 1250ms</td>
        </tr>
        <tr>
          <td>15:30:32</td>
          <td>search</td>
          <td>✅ 320ms</td>
        </tr>
        <tr>
          <td>15:30:18</td>
          <td>startup</td>
          <td>❌ 1850ms</td>
        </tr>
      </table>
    </div>

    <!-- Widget 6: Performance Score -->
    <div class="widget">
      <h3>Overall Performance Score</h3>
      <div class="metric-value" style="color: #f57c00;">78/100</div>
      <div class="trend down">📉 -5 points from yesterday</div>
      <progress value="78" max="100" style="width: 100%;"></progress>
    </div>
  </div>

  <script>
    // WebSocket connection for real-time updates
    const ws = new WebSocket('ws://localhost:3000/performance-stream');

    ws.onmessage = (event) => {
      const data = JSON.parse(event.data);
      updateDashboard(data);
    };

    function updateDashboard(data) {
      // Update widgets in real-time
      document.getElementById('lastUpdate').textContent = new Date().toLocaleString();
      // ... update other widgets
    }

    // Auto-refresh every 5 seconds
    setInterval(() => {
      ws.send(JSON.stringify({ type: 'refresh' }));
    }, 5000);
  </script>
</body>
</html>
```

#### Console - Dashboard Server
```bash
$ npx playwright-perf dashboard --port 3000

🎭 Playwright Performance Dashboard

Server started: http://localhost:3000
WebSocket: ws://localhost:3000/performance-stream

📊 Dashboard Features:
  ✅ Real-time performance monitoring
  ✅ Live test execution tracking
  ✅ Performance trends and alerts
  ✅ Budget compliance tracking

Press Ctrl+C to stop

[15:30:01] New connection from 127.0.0.1
[15:30:05] Test started: checkout.spec.ts (worker 0)
[15:30:07] Performance sample: startup_GH = 1250ms
[15:30:09] Test completed: checkout.spec.ts (✅ PASS)
[15:30:12] Alert triggered: startup_GH exceeds threshold
```

---

## Summary Table

| Feature | Fidelity | Complexity | Estimated Effort |
|---------|----------|------------|------------------|
| Baseline Comparison | 🟢 High | Medium | 2-3 days |
| Percentile Statistics | 🟢 High | Low | 1-2 days |
| Error Messages | 🟢 High | Low | 1 day |
| Performance Budgets | 🟢 High | Medium | 2-3 days |
| Unit Tests | 🟢 High | Medium | 2-3 days |
| Trend Analysis | 🟡 Medium | Medium-High | 3-4 days |
| Enhanced Charts | 🟡 Medium | Medium | 3-4 days |
| CI/CD Integration | 🟢 High | Medium | 2-3 days |
| Stream Processing | 🟡 Medium | High | 4-5 days |
| Documentation | 🟡 Medium | Low | 2-3 days |
| Custom Metrics | 🟡 Medium | Medium-High | 3-5 days |
| Distributed Tracing | 🟠 Low | High | 5-7 days |
| Branch Comparison | 🟡 Medium | Medium-High | 3-4 days |
| Plugin Marketplace | 🟠 Low | Very High | 10-15 days |
| Real-time Dashboard | 🟠 Low | Very High | 7-10 days |

---

**Note:** All examples are designed to illustrate the feature capabilities. Implementation details may vary based on technical constraints and user feedback.
