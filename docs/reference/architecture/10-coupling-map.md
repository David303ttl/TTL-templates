<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Coupling Map

> Document ID: `PROJECT-ARCH-10`
> Phase: R-10
> Status: Draft

## 1. Module-to-Module Coupling

| Source Module | Target Module | Coupling Type | Coupling Mechanism | Strength | File References |
|---|---|---|---|---|---|

## 2. Global Variable Access Map

| Global Variable | Type | Owner (Writer) | Readers | Volatile? | Sync Method | ISR-Shared? |
|---|---|---|---|---|---|---|

## 3. Buffer Ownership Map

| Buffer | Size | Writer Context | Reader Context | Sync Method | DMA? | Failure Mode |
|---|---|---|---|---|---|---|

## 4. Listener / Observer Chains

| Event Source | Listener | Registration Point | Call Order | Purpose |
|---|---|---|---|---|

## 5. ISR and Main Loop Shared State

| Variable | ISR Writer | Main Loop Reader | ISR Reader | Main Loop Writer | Volatile? | Risk |
|---|---|---|---|---|---|---|

## 6. Blast Radius Table

| Change Target | Affected Modules | Affected Globals | Affected ISRs | Severity |
|---|---|---|---|---|

## 7. Safe Change Ordering

1. Leaves-first changes - modules with only incoming edges
2. Intermediate changes - modules with controlled blast radius
3. Core changes - modules with highest coupling, done last with maximum verification

## 8. Revision History

| Version | Date | Notes |
|---|---|---|


