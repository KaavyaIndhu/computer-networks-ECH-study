# When ECH Fails: Interpreting Client-Side Evidence of Encrypted ClientHello Connection Failures

A research project studying why **Encrypted ClientHello (ECH)** connections fail and how client-side evidence can help distinguish observable failure outcomes without assuming an unverified cause.

**Authors:** Alisha Ranjan Kumar and Kaavya Indhu  
**Institution:** Department of Computer Science and Engineering, Vellore Institute of Technology, Vellore, India

## Overview

Encrypted ClientHello (ECH) is a TLS mechanism that protects sensitive information in the connection handshake, including the requested server name.

However, an ECH connection failure does not automatically indicate censorship or network interference. It can also result from configuration problems, server rejection, DNS-related behaviour, or transport errors.

This project investigates ECH failures using paired HTTPS measurements, configuration-focused follow-up tests, and a controlled local testbed.

## Objectives

- Compare ECH-enabled requests with conventional HTTPS controls.
- Examine client-side logs and ECH rejection and retry-configuration indicators.
- Investigate how configuration choices affect connection outcomes.
- Test successful ECH negotiation and malformed configurations in a local environment.
- Preserve uncertainty when the available evidence cannot establish the root cause.

## Methodology

**1. Paired measurements**

Four campaigns collected 240 records, forming 120 pairs of ECH-enabled and conventional HTTPS requests across three endpoints. The campaigns used Cloudflare and Google DNS-over-HTTPS (DoH) resolvers.

**2. Configuration experiments**

Follow-up tests examined DNS-published ECH configurations and server-provided retry configurations to investigate differences in connection outcomes.

**3. Controlled testbed**

A local TLS server and custom curl client were used to observe successful ECH negotiation, malformed-configuration errors, and rejection with retry information.

## Key Results

| Measurement | Result |
|---|---:|
| Total records | 240 |
| Complete request pairs | 120 |
| Successful ECH attempts | 80 (66.7%) |
| ECH connection errors | 40 (33.3%) |
| Successful conventional HTTPS controls | 120 (100%) |

All 40 observed ECH connection errors occurred at `defo.ie`. ECH succeeded in every recorded attempt at the other two endpoints.

A fresh server-provided retry configuration succeeded in follow-up tests where the DNS-published configuration failed. This suggests configuration freshness or compatibility may be relevant, but the original failure's root cause remains unresolved.

**Important:** These results do not establish censorship, DNS tampering, or interference by an Indian network operator. They describe only the tested endpoints and conditions.

## Technology Stack

- Python 3.10+
- TLS and Encrypted ClientHello (ECH)
- curl
- DNS-over-HTTPS (DoH)
- pytest
- JSONL and CSV for measurement data

## Repository Structure

```text
computer_networks_project/
├── data/
├── experiments/
│   └── controlled_ech/
├── paper/
├── results/
├── src/
│   └── ech_study/
├── tests/
├── pyproject.toml
└── README.md
```

- `src/` — shared measurement schema and Python package.
- `experiments/` — controlled ECH experiments and analysis scripts.
- `data/` — measurement data.
- `results/` — experimental outputs.
- `tests/` — automated tests.
- `paper/` — research paper and supporting material.

## Installation

Clone the repository:

```bash
git clone https://github.com/alisha873/computer_networks_project.git
cd computer_networks_project
```

Create a virtual environment and install the project:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -e ".[dev]"
```

Run the test suite:

```bash
python -m pytest
```

The project requires Python 3.10 or newer. The development dependencies include pytest.

## Limitations

- The measurements cover only three public endpoints.
- All observed ECH connection errors occurred at one endpoint.
- The physical access network was not independently verified.
- Some campaign timestamps do not match dates in filenames.
- Follow-up experiments do not conclusively establish the original failure's cause.
- The controlled testbed is not a comprehensive accuracy benchmark for failure classification.

## Conclusion

The study demonstrates that **detecting a connection failure is different from identifying its cause**. Paired controls, client-side logs, configuration experiments, and a controlled testbed help interpret observed outcomes, but conclusions must remain limited to what the evidence supports.

## Research Paper

The complete paper, *When ECH Fails: Interpreting Client-Side Evidence of Encrypted ClientHello Connection Failures*, provides the detailed methodology, experimental tables, discussion, and references.

See the `paper/` directory for the research paper.

## Authors

- Alisha Ranjan Kumar
- Kaavya Indhu

Department of Computer Science and Engineering, Vellore Institute of Technology, Vellore, India.
