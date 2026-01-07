<h1 align="center">Business Questions — 100cep Gateway</h1>

This document lists all business questions to be answered by the Data Engineering MVP. The questions are organized by category and aim to provide strategic insights for 100cep Gateway's operation.

---

## 📊 Analysis Categories

### 1️⃣ Payment Behavior
**Objective**: Understand customers' payment preferences and identify usage patterns.

**Question 1**: What is the most used payment method by 100cep Gateway's customers?

**Context**: This analysis makes it possible to identify the most popular payment methods among customers, supporting strategic decisions about:
- Negotiation of fees with acquirers
- Prioritization of integrations with card brands
- Investment in processing infrastructure
- Marketing strategies focused on specific methods

---

### 2️⃣ Financial Performance
**Objective**: Monitor the evolution of revenue and identify seasonality.

**Question 2**: What is the revenue history for the year 2017?

**Context**: Tracking revenue over time allows you to:
- Identify seasonal periods of high/low demand
- Plan operational resources
- Project future growth
- Assess the effectiveness of commercial campaigns

---

### 3️⃣ Risk and Chargebacks
**Objective**: Assess the level of exposure to chargebacks and develop mitigation strategies.

**Question 3**: What is the proportion of orders with and without chargeback requests?

**Context**: Understanding the chargeback rate is essential to:
- Assess the operational risk of the gateway
- Implement antifraud rules
- Provision financial reserves
- Negotiate terms with acquirers

---

**Question 4**: Which payment methods have the highest chargeback risk?

**Context**: Risk analysis by payment method makes it possible to:
- Define differentiated risk analysis rules
- Adjust pricing (MDR) by method
- Implement additional checks for high-risk methods
- Educate sellers on safe practices

---

### 4️⃣ Geographic Analysis
**Objective**: Identify regions with higher risk and expansion opportunities.

**Question 5**: Which states have the highest chargeback rates?

**Context**: The geographic distribution of chargebacks makes it possible to:
- Identify higher-risk regions
- Implement analysis rules by location
- Plan regional expansion strategies
- Understand fraud patterns by region

---

## 🎯 Practical Applications

The answers to these questions will be used for:

1. **Executive Dashboard**: Consolidated visualization of key KPIs
2. **Automatic Alerts**: Notifications when metrics exceed thresholds
3. **Predictive Model**: Basis for machine learning in fraud prevention
4. **Regulatory Reports**: Data for compliance and audit
5. **Business Intelligence**: Insights for strategic decision-making

---

## 📈 Data Layer

The analyses will be carried out using tables from the **Gold layer**:
- `fato_transacoes`: Consolidated transactional data
- `dim_chargebacks`: Information about disputes
- `dim_geolocalizacao`: Geographic data for regional analysis
- `dim_data`: Time dimension for historical analyses
- `dim_clientes` and `dim_vendedores`: Customer and seller profiles

---