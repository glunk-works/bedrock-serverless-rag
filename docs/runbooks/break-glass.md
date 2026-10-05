# Break-Glass Emergency Access Runbook

## Overview
Procedure for invoking elevated break-glass credentials during critical infrastructure outages or credential invalidation.

## Activation Steps
1. Request one-time break-glass token from AWS SSM Parameter Store.
2. Assume the dedicated `EmergencyBreakGlassRole` with explicit MFA session.
3. Perform remediation, export incident logs, and revoke the session token.
