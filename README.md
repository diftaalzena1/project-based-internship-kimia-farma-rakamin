# PT.-Kimia-Farma-Performance-Analytics

## About The Program
The Project-Based Internship program, a collaboration between Rakamin Academy and Kimia Farma in Big Data Analytics, is designed to accelerate the career development of individuals interested in pursuing a career as Big Data Analysts at Kimia Farma. This program includes fundamental learning resources like Article Reviews and Company Coaching Videos to help participants understand the skills and competencies needed for Big Data Analytics professionals. Additionally, participants will undergo assessments in the form of weekly tasks, culminating in the creation of a final project that serves as a portfolio showcasing the skills learned throughout the program.

## About Kimia Farma
Kimia Farma is Indonesia's first pharmaceutical company, established in 1817 by the Dutch East Indies government. Initially known as NV Chemicalien Handle Rathkamp & Co, the company underwent nationalization during the early independence years. In 1958, the Indonesian government merged several pharmaceutical firms to form PNF (State Pharmaceutical Company) Bhinneka Kimia Farma. In 1971, the company’s legal status was changed to a Limited Liability Company, and its name was revised to PT Kimia Farma (Persero).

## Objective
The project aims to assess Kimia Farma's business performance from 2020 to 2023. The tasks include importing datasets into BigQuery, creating analysis tables, and building a performance dashboard using Google Looker Studio.

## Challenges
Challenge 1: Import four provided datasets into BigQuery.
Challenge 2: Create an analytical table in BigQuery by merging the datasets.
Challenge 3: Develop a performance dashboard in Looker Studio, visualizing the data from the analytical table in BigQuery.

## Datasets
The following datasets were used for this analysis:
kf_final_transaction
kf_inventory
kf_kantor_cabang
kf_product

## SQL Syntax
The following SQL syntax was used to create a new table, kf_analisa, in the kimia_farma database by joining data from existing tab
CREATE TABLE kimia_farma.kf_analisa AS
SELECT
  ft.transaction_id,
  ft.date,
  ft.branch_id,
  kc.branch_name,
  kc.kota,
  kc.provinsi,
  kc.rating AS rating_cabang,
  ft.product_id,
  p.product_name,
  ft.price,
  ft.discount_percentage,
  CASE 
    WHEN ft.price <= 50000 THEN 0.1
    WHEN ft.price > 50000 AND ft.price <= 100000 THEN 0.15
    WHEN ft.price > 100000 AND ft.price <= 300000 THEN 0.2
    WHEN ft.price > 300000 AND ft.price <= 500000 THEN 0.25
    ELSE 0.3
  END AS gross_profit_percentage,
  (ft.price * (1 - (ft.discount_percentage / 100))) AS nett_sales,
  (ft.price * (1 - (ft.discount_percentage / 100)) * 
    CASE 
      WHEN ft.price <= 50000 THEN 0.1
      WHEN ft.price > 50000 AND ft.price <= 100000 THEN 0.15
      WHEN ft.price > 100000 AND ft.price <= 300000 THEN 0.2
      WHEN ft.price > 300000 AND ft.price <= 500000 THEN 0.25
      ELSE 0.3
    END) AS nett_profit,
  ft.rating AS rating_transaksi
FROM `rakamin-kf-analytics-030325.kimia_farma.kf_final_transaction` AS ft
JOIN `rakamin-kf-analytics-030325.kimia_farma.kf_kantor_cabang` AS kc
  ON ft.branch_id = kc.branch_id
JOIN `rakamin-kf-analytics-030325.kimia_farma.kf_product` AS p
  ON ft.product_id = p.product_id;

It includes various transformations:
- Gross profit percentage is calculated based on product price.
- Nett sales and nett profit are calculated considering the discount percentage.
- Ratings from both the branch (rating_cabang) and transaction (rating_transaksi) are included.

