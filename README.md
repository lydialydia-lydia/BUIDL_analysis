# BUIDL_analysis

## Overview
This notebook builds a lightweight issuer liquidity risk framework for the BUIDL token on Ethereum. It uses on-chain Mint/Burn activity as a proxy for subscriptions/redemptions, calibrates tail redemption shocks from historical burn rates, and runs an issuer-side ALM + gating/queue simulation to quantify delayed redemptions under operational constraints.

## Data
	•	Source: Dune-exported weekly series of BUIDL (Ethereum) Mint/Burn


## Method (Notebook Sections)
A) On-chain Flow Monitoring
	•	Plot weekly Mint vs Burn and net flow (Mint − Burn)
	•	Identify top burn weeks (tail redemption proxy)

B) Shock Calibration (p95/p99/max)
	•	Construct a burn-rate series: burn_rate = Burn / start-of-week supply (proxy)
	•	Calibrate scenarios: mild (p95), stress (p99), crisis (max)
	•	Save shocks to shocks.json and visualize burn-rate distribution (hist + CDF)

C) Issuer ALM + Gate/Queue Simulation
	•	Simulate weekly redemption demand, cash buffer usage, asset liquidation capacity, settlement lag, haircut, and gating rules
	•	Outputs: queue trajectory over time and KPIs:
	•	max_queue_pct (peak queued redemptions as %AUM)
	•	weeks_gated (weeks with gating/queue active)
	•	queue_end_pct (end-of-horizon remaining queue)

## Key Outputs
	•	Time-series plots: queue % over time for mild/stress/crisis scenarios
	•	KPI summary table comparing liquidity outcomes across scenarios

## Assumptions & Limitations
	•	Burn is a proxy for redemption pressure; some burns may be non-economic (e.g., operational flows).
	•	AUM/supply is proxied using cumulative net flows (Mint − Burn); ideally replace with on-chain totalSupply or reported AUM.
	•	Shock duration is set to shock_weeks=1 (episodic tail-week stress); results are sensitive to longer shock windows.
