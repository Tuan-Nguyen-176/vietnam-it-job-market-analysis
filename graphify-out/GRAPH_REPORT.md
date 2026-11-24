# Graph Report - vietnam-it-job-market-analysis  (2026-11-24)

## Corpus Check
- 71 files · ~75,582 words
- Verdict: corpus is large enough that graph structure adds value.
- Unclassified: 52 file(s) not represented in the graph (top: .csv 34, .jsonl 12, .ipynb 5)
- WARNING: 52 of 123 files seen (42%) have no supported extractor and are missing from this graph entirely. It may look complete while representing only part of the codebase.

## Summary
- 804 nodes · 1533 edges · 46 communities (43 shown, 3 thin omitted)
- Extraction: 85% EXTRACTED · 14% INFERRED · 1% AMBIGUOUS · INFERRED: 215 edges (avg confidence: 0.86)
- Token cost: 2,911,362 input · 0 output

## Community Hubs (Navigation)
- Project Docs & Pipeline Overview
- Step Mini-Notes & Shared Sample
- Slide Outline (18 slides)
- Listing Crawlers
- Assignment Brief Requirements
- Report PDF Findings
- Detail Fetch Extractors
- JSONL I/O & IT Filter
- Field Normalization (lam_sach)
- EDA Skill Charts
- Report: Funnel & Salary Handling
- Association Rules (2.4)
- K-Means Clustering (2.3)
- CSV Export
- Lift Heatmaps
- Salary Classification (2.5)
- Tuấn's 2.1/2.2 Slides
- Salary Parsing
- Cross-Source Job Grouping
- PCA Cluster Chart (slides 2.1/2.2)
- PCA Cluster Chart (slides 2.3)
- Đức's Crawler/Pipeline Slides
- IT General Skill Heatmap
- Cluster Size Chart (slides 2.1/2.2)
- Cluster Size Chart (slides 2.3)
- Pruned Tree Figure (output)
- Intra-Site Dedup
- Rules Scatter (output)
- Style Ref: KPI Cards
- Style Ref: Timeline Slide
- Style Ref: Progress Slide
- Socratic Tutor Skill
- Pruning Curve (output)
- Data/AI Skill Heatmap
- Salary by Level Boxplot
- Rules Scatter (slides)
- Pruned Tree Figure (slides)
- Pruning Curve (slides)
- RF Confusion Counts (output)
- Elbow/Silhouette (slides 2.3)
- RF Confusion Counts (slides)
- RF Normalized Confusion
- RF Recall by Band
- Region Comparison Chart
- Elbow/Silhouette (slides 2.1/2.2)
- Skill Dictionary & Extraction

## God Nodes (most connected - your core abstractions)
1. `Báo cáo Bài tập lớn Nhóm 4 (Nhom04_BaoCao_BaiTapLon.md)` - 74 edges
2. `CLAUDE.md - project guide (KDD pipeline, progress, checklist)` - 31 edges
3. `normalize()` - 22 edges
4. `Nhom04 Sườn Slide (18-slide presentation outline)` - 20 edges
5. `PIPELINE_DOCS.md - cách nhóm làm sạch dữ liệu` - 19 edges
6. `K-Means K = 8 trên 2.218 tin × 143 kỹ năng` - 19 edges
7. `write_jsonl()` - 18 edges
8. `CRAWLER_DOCS.md - giải thích thư mục crawler/` - 18 edges
9. `read_jsonl()` - 16 edges
10. `clean_record()` - 15 edges

## Surprising Connections (you probably didn't know these)
- `Checkpoint full.jsonl mỗi 20 tin qua file .tmp + replace` --semantically_similar_to--> `K3 interactive_scrape ghi file không nguyên tử`  [INFERRED] [semantically similar]
  crawler/CRAWLER_DOCS.md → CLAUDE.md
- `Cluster naming by lift and skill signatures` --semantically_similar_to--> `Support / Confidence / Lift / Conviction measures`  [INFERRED] [semantically similar]
  buoc 2.3/miniNote_buoc2.3_GomCum.md → buoc2.4/bao_cao_buoc_2.4.md
- `Stability check over 20 random seeds` --semantically_similar_to--> `Denominator sensitivity (1,225/1,228 rules survive on 2,843 jobs)`  [INFERRED] [semantically similar]
  buoc 2.3/miniNote_buoc2.3_GomCum.md → buoc2.4/bao_cao_buoc_2.4.md
- `Denominator sensitivity (1,225/1,228 rules survive on 2,843 jobs)` --semantically_similar_to--> `Robustness: skill rank by company count and by source`  [INFERRED] [semantically similar]
  buoc2.4/bao_cao_buoc_2.4.md → buoc2.1_2.2/miniNote_buoc2.2_EDA.md
- `Ý tưởng slide phần Đức (Crawler + Pipeline, Bước 2.3)` --cites--> `Báo cáo Bài tập lớn Nhóm 4 (Nhom04_BaoCao_BaiTapLon.md)`  [AMBIGUOUS]
  slide thuyet trinh/Nhom04_YTuongSlide_Duc.md → file báo cáo bài nhóm/Nhom04_BaoCao_BaiTapLon.md

## Import Cycles
- None detected.

## Hyperedges (group relationships)
- **Luồng pipeline raw → dedup → it → full → salary/skills → CSV** — pipline_pipeline_docs_duplicate_py, pipline_pipeline_docs_filter_it_py, crawler_crawler_docs_fetch_detail_py, pipline_pipeline_docs_clean_salary_py, pipline_pipeline_docs_extract_tech_skill_py, pipline_pipeline_docs_export_data_py [EXTRACTED 1.00]
- **5 crawler cùng xuất schema chung** — crawler_crawler_docs_itviec_py, crawler_crawler_docs_topdev_py, crawler_crawler_docs_vietnamnetworks_py, crawler_crawler_docs_topcv_py, crawler_crawler_docs_linkedin_py, giai_trinh_key_common_record_schema [EXTRACTED 1.00]
- **Hệ khóa truy vết và nối bảng (job_uid, URL_Goc, job_group_id/is_primary)** — data_dictionary_job_uid, data_dictionary_url_goc, data_dictionary_job_group_id_is_primary [EXTRACTED 1.00]
- **Association rule mining pipeline (2.4)** — buoc2_4_bao_cao_buoc_2_4_transaction_encoder, buoc2_4_bao_cao_buoc_2_4_skill_baskets, buoc2_4_bao_cao_buoc_2_4_apriori_fpgrowth, buoc2_4_bao_cao_buoc_2_4_rule_filter_thresholds, buoc2_4_bao_cao_buoc_2_4_redundant_rule_removal, buoc2_4_bao_cao_buoc_2_4_output_rule_csvs [EXTRACTED 1.00]
- **Leakage-safe salary classification flow (2.5)** — buoc_2_5_portable_mininote_2_5_scope_filter_761_688, buoc_2_5_portable_mininote_2_5_features_skills_experience, buoc_2_5_portable_mininote_2_5_stratified_group_kfold, buoc_2_5_portable_mininote_2_5_decision_tree_ccp_alpha, buoc_2_5_portable_mininote_2_5_random_forest, buoc_2_5_portable_mininote_2_5_dummy_baseline, buoc_2_5_portable_mininote_2_5_confusion_matrix_metrics [EXTRACTED 1.00]
- **Shared denominator: included_provisional + skills from clean_data_processed.csv** — buoc2_1_2_2_mininote_buoc2_1_cleaning_clean_data_processed_csv, buoc2_1_2_2_mininote_buoc2_1_cleaning_scope_status, buoc2_1_2_2_mininote_buoc2_1_cleaning_skills_column, buoc_2_3_mininote_buoc2_3_gomcum_skill_thresholds, buoc2_4_bao_cao_buoc_2_4_skill_baskets, buoc_2_5_portable_mininote_2_5_scope_filter_761_688 [INFERRED 0.85]
- **Quy trình KDD từ cào dữ liệu đến phân lớp** — file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_crawler_hai_giai_doan, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_pheu_du_lieu, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_chuan_hoa_luong_regex, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_clean_data_processed, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_kmeans_k8, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_apriori, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_decision_tree_ccp_alpha [EXTRACTED 1.00]
- **8 phân khúc kỹ năng của K-Means K = 8** — file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_cum_frontend_web, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_cum_du_lieu_doanh_nghiep, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_cum_backend_java, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_cum_devops_cloud, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_cum_data_science_ml_llm, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_cum_he_thong_mang_bao_mat, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_cum_agile_scrum, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_cum_c_cpp_linux [EXTRACTED 1.00]
- **So sánh Dummy, Decision Tree, Random Forest trên cùng fold nhóm** — file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_dummy_baseline, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_decision_tree_ccp_alpha, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_random_forest, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_stratified_group_kfold [EXTRACTED 1.00]
- **KDD pipeline steps 2.1-2.5** — baitaplon_buoc_2_1_data_cleaning, baitaplon_buoc_2_2_eda, baitaplon_buoc_2_3_clustering, baitaplon_buoc_2_4_association_rules, baitaplon_buoc_2_5_classification [EXTRACTED 1.00]
- **Crawler technical requirements (pagination, dynamic loading, random delay)** — baitaplon_pagination, baitaplon_dynamic_loading, baitaplon_random_delay, baitaplon_giai_doan_1_web_scraping [EXTRACTED 1.00]
- **Required submission files** — baitaplon_scraper_py, baitaplon_raw_data_csv, baitaplon_clean_data_csv, baitaplon_mining_pipeline_notebook, baitaplon_bao_cao_pdf, baitaplon_slide_bao_cao [EXTRACTED 1.00]
- **Data funnel from raw crawl to analysis set** — nhom04_suonslide_slide3_crawler_5_sources, nhom04_suonslide_it_filter_rules, nhom04_suonslide_is_primary_dedup, nhom04_suonslide_scope_status, nhom04_suonslide_slide4_data_funnel [EXTRACTED 1.00]
- **Misleading association-rule traps** — nhom04_suonslide_rule_kubernetes_docker, nhom04_suonslide_rule_python_sql_weak, nhom04_suonslide_conviction_infinite, nhom04_suonslide_redundant_rule_removal [EXTRACTED 1.00]
- **Salary-band model comparison** — nhom04_suonslide_dummy_baseline, nhom04_suonslide_decision_tree_ccp_alpha, nhom04_suonslide_random_forest_200, nhom04_suonslide_pruning_path_survey [EXTRACTED 1.00]
- **Pipeline tiền xử lý tạo clean_data.csv** — file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_salary_regex, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_lam_sach_normalization, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_skill_extraction, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_group_jobs, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_clean_data_csv [EXTRACTED 1.00]
- **Bốn bước khai phá dùng chung clean_data_processed.csv (2.843 tin có JD)** — file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_eda, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_kmeans_clustering, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_association_rules, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_classification, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_clean_data_processed [EXTRACTED 1.00]
- **So sánh mô hình phân lớp (Bảng 5.3)** — file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_dummy_baseline, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_decision_tree_ccp, file_báo_cáo_bài_nhóm_nhom04_baocao_baitaplon_random_forest [EXTRACTED 1.00]
- **Three salary-band target classes** — buoc_2_5_portable_output_salary_scope_figures_pruned_tree_top_levels_class_duoi_15, buoc_2_5_portable_output_salary_scope_figures_pruned_tree_top_levels_class_15_den_30, buoc_2_5_portable_output_salary_scope_figures_pruned_tree_top_levels_class_tren_30 [EXTRACTED 1.00]
- **ccp_alpha selection by 3-fold CV to control overfitting** — buoc_2_5_portable_output_salary_scope_figures_pruning_curve_ccp_alpha, buoc_2_5_portable_output_salary_scope_figures_pruning_curve_three_fold_cv, buoc_2_5_portable_output_salary_scope_figures_pruning_curve_selected_alpha_0_0081, buoc_2_5_portable_output_salary_scope_figures_pruning_curve_train_vs_validation_gap [INFERRED 0.85]
- **DevOps/Cloud skills co-occur strongly (high lift cluster)** — buoc2_4_output_heatmap_lift_docker_kubernetes_pair, buoc2_4_output_heatmap_lift_aws_azure_pair, buoc2_4_output_heatmap_lift_devops_kubernetes_pair, buoc2_4_output_heatmap_lift_devops_cloud_skill_block [INFERRED 0.85]
- **Association rule filter thresholds shown in chart** — buoc2_4_output_scatter_support_confidence_support_floor_0_03, buoc2_4_output_scatter_support_confidence_confidence_floor_0_5, buoc2_4_output_scatter_support_confidence_lift_colorscale, buoc2_4_output_scatter_support_confidence_nonredundant_rules [INFERRED 0.85]
- **Cloud/DevOps skill profile changes with seniority** — slide_thuyet_trinh_buoc_2_1_va_2_2_2_2_heatmap_cloud_devops_devops_cicd_core, slide_thuyet_trinh_buoc_2_1_va_2_2_2_2_heatmap_cloud_devops_senior_shift_aws_terraform, slide_thuyet_trinh_buoc_2_1_va_2_2_2_2_heatmap_cloud_devops_junior_hands_on_tools [INFERRED 0.85]
- **Senior/Lead postings concentrate on cloud, security, ERP, LLM skills** — slide_thuyet_trinh_buoc_2_1_va_2_2_2_2_heatmap_it_tong_quat_cloud_devops_senior_shift, slide_thuyet_trinh_buoc_2_1_va_2_2_2_2_heatmap_it_tong_quat_cybersecurity_lead_peak, slide_thuyet_trinh_buoc_2_1_va_2_2_2_2_heatmap_it_tong_quat_erp_lead_peak, slide_thuyet_trinh_buoc_2_1_va_2_2_2_2_heatmap_it_tong_quat_llm_rise [INFERRED 0.75]
- **Phân tích kỹ năng theo cấp bậc trong nhóm Phần mềm/Web (EDA 2.2.5)** — slide_thuyet_trinh_buoc_2_1_va_2_2_2_2_heatmap_phan_mem_web_software_web_role_group, slide_thuyet_trinh_buoc_2_1_va_2_2_2_2_heatmap_phan_mem_web_seniority_levels, slide_thuyet_trinh_buoc_2_1_va_2_2_2_2_heatmap_phan_mem_web_skill_mention_rate, slide_thuyet_trinh_buoc_2_1_va_2_2_2_2_heatmap_phan_mem_web_top_skills_listed [INFERRED 0.85]
- **Salary by seniority analysis** — slide_thuyet_trinh_buoc_2_1_va_2_2_2_2_luong_theo_cap_bac_chart, slide_thuyet_trinh_buoc_2_1_va_2_2_2_2_luong_theo_cap_bac_salary_midpoint_luong_tb, slide_thuyet_trinh_buoc_2_1_va_2_2_2_2_luong_theo_cap_bac_seniority_levels, slide_thuyet_trinh_buoc_2_1_va_2_2_2_2_luong_theo_cap_bac_lead_below_senior_anomaly [INFERRED 0.85]
- **Chọn K bằng Elbow + Silhouette + quy tắc ổn định** — slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_elbow_silhouette_elbow_inertia_curve, slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_elbow_silhouette_silhouette_curve, slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_elbow_silhouette_k_final_8 [EXTRACTED 1.00]
- **Eight-cluster partition of IT job skill profiles** — slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_kich_thuoc_cum_cluster_frontend_web, slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_kich_thuoc_cum_cluster_du_lieu_doanh_nghiep, slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_kich_thuoc_cum_cluster_backend_java, slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_kich_thuoc_cum_cluster_devops_cloud, slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_kich_thuoc_cum_cluster_data_science_ml_llm, slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_kich_thuoc_cum_cluster_he_thong_mang_bao_mat, slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_kich_thuoc_cum_cluster_agile_scrum, slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_kich_thuoc_cum_cluster_c_cpp_linux [EXTRACTED 1.00]
- **Eight named K-Means skill clusters shown in PCA 2D** — slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_pca_2d_frontend_web, slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_pca_2d_du_lieu_doanh_nghiep, slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_pca_2d_backend_java, slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_pca_2d_devops_cloud, slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_pca_2d_data_science_ml_llm, slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_pca_2d_he_thong_mang_bao_mat, slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_pca_2d_agile_scrum, slide_thuyet_trinh_buoc_2_1_va_2_2_2_3_pca_2d_c_cpp_linux [EXTRACTED 1.00]
- **K selection: Elbow + Silhouette + rule -> K = 8** — slide_thuyet_trinh_buoc_2_3_2_3_elbow_silhouette_elbow_inertia_curve, slide_thuyet_trinh_buoc_2_3_2_3_elbow_silhouette_silhouette_curve, slide_thuyet_trinh_buoc_2_3_2_3_elbow_silhouette_k_rule_candidates, slide_thuyet_trinh_buoc_2_3_2_3_elbow_silhouette_k_final_8 [INFERRED 0.85]
- **Eight named skill clusters from K-Means K=8** — slide_thuyet_trinh_buoc_2_3_2_3_kich_thuoc_cum_cluster_frontend_web, slide_thuyet_trinh_buoc_2_3_2_3_kich_thuoc_cum_cluster_du_lieu_doanh_nghiep, slide_thuyet_trinh_buoc_2_3_2_3_kich_thuoc_cum_cluster_backend_java, slide_thuyet_trinh_buoc_2_3_2_3_kich_thuoc_cum_cluster_devops_cloud, slide_thuyet_trinh_buoc_2_3_2_3_kich_thuoc_cum_cluster_data_science_ml_llm, slide_thuyet_trinh_buoc_2_3_2_3_kich_thuoc_cum_cluster_he_thong_mang_bao_mat, slide_thuyet_trinh_buoc_2_3_2_3_kich_thuoc_cum_cluster_agile_scrum, slide_thuyet_trinh_buoc_2_3_2_3_kich_thuoc_cum_cluster_c_cpp_linux [EXTRACTED 1.00]
- **Eight named K-Means skill clusters** — slide_thuyet_trinh_buoc_2_3_2_3_pca_2d_cluster_frontend_web, slide_thuyet_trinh_buoc_2_3_2_3_pca_2d_cluster_du_lieu_doanh_nghiep, slide_thuyet_trinh_buoc_2_3_2_3_pca_2d_cluster_backend_java, slide_thuyet_trinh_buoc_2_3_2_3_pca_2d_cluster_devops_cloud, slide_thuyet_trinh_buoc_2_3_2_3_pca_2d_cluster_data_science_ml_llm, slide_thuyet_trinh_buoc_2_3_2_3_pca_2d_cluster_he_thong_mang_bao_mat, slide_thuyet_trinh_buoc_2_3_2_3_pca_2d_cluster_agile_scrum, slide_thuyet_trinh_buoc_2_3_2_3_pca_2d_cluster_c_cpp_linux [EXTRACTED 1.00]
- **DevOps/Cloud skill co-occurrence (lift > 2)** — slide_thuyet_trinh_buoc_2_4_heatmap_lift_docker_kubernetes_pair, slide_thuyet_trinh_buoc_2_4_heatmap_lift_aws_azure_pair, slide_thuyet_trinh_buoc_2_4_heatmap_lift_devops_kubernetes_pair, slide_thuyet_trinh_buoc_2_4_heatmap_lift_devops_cloud_skill_block [INFERRED 0.85]
- **Rule filter metrics shown in scatter (support, confidence, lift)** — slide_thuyet_trinh_buoc_2_4_scatter_support_confidence_support_axis_min_support_0_03, slide_thuyet_trinh_buoc_2_4_scatter_support_confidence_confidence_axis_min_0_5, slide_thuyet_trinh_buoc_2_4_scatter_support_confidence_lift_color_scale, slide_thuyet_trinh_buoc_2_4_scatter_support_confidence_nonredundant_rules [INFERRED 0.85]
- **Pruned tree predicts salary band from experience features only** — slide_thuyet_trinh_buoc_2_5_1795496660122_1142875591668223248_g2571233605694853986_c6cdb5285d880c2053935dfb299cc52f_split_exp_min_years, slide_thuyet_trinh_buoc_2_5_1795496660122_1142875591668223248_g2571233605694853986_c6cdb5285d880c2053935dfb299cc52f_split_missingindicator_exp_max_years, slide_thuyet_trinh_buoc_2_5_1795496660122_1142875591668223248_g2571233605694853986_c6cdb5285d880c2053935dfb299cc52f_salary_band_classes, slide_thuyet_trinh_buoc_2_5_1795496660122_1142875591668223248_g2571233605694853986_c6cdb5285d880c2053935dfb299cc52f_pruned_decision_tree_figure [INFERRED 0.85]
- **Pruning alpha selection by CV macro F1** — slide_thuyet_trinh_buoc_2_5_1795496660123_1142875591668223248_g2571233605694853986_ad7a1e82ddb8f5c28719f2d7554d0330_ccp_alpha_cost_complexity_pruning, slide_thuyet_trinh_buoc_2_5_1795496660123_1142875591668223248_g2571233605694853986_ad7a1e82ddb8f5c28719f2d7554d0330_macro_f1_metric, slide_thuyet_trinh_buoc_2_5_1795496660123_1142875591668223248_g2571233605694853986_ad7a1e82ddb8f5c28719f2d7554d0330_selected_alpha_0_0081, slide_thuyet_trinh_buoc_2_5_1795496660123_1142875591668223248_g2571233605694853986_ad7a1e82ddb8f5c28719f2d7554d0330_overfitting_gap_train_validation [INFERRED 0.85]
- **KPI card anatomy: big number + mini chart + legend + note** — slide_thuyet_trinh_kiểu_slide_screenshot_2026_11_24_133659_three_kpi_card_layout, slide_thuyet_trinh_kiểu_slide_screenshot_2026_11_24_133659_waterfall_chart, slide_thuyet_trinh_kiểu_slide_screenshot_2026_11_24_133659_stacked_bar_breakdown, slide_thuyet_trinh_kiểu_slide_screenshot_2026_11_24_133659_restrained_palette [INFERRED 0.85]
- **Assertion-headline slide layout: kicker title, timeline evidence, KPI cards, sourced footer** — slide_thuyet_trinh_kiểu_slide_screenshot_2026_11_24_133754_eyebrow_kicker_title, slide_thuyet_trinh_kiểu_slide_screenshot_2026_11_24_133754_horizontal_milestone_timeline, slide_thuyet_trinh_kiểu_slide_screenshot_2026_11_24_133754_kpi_stat_cards, slide_thuyet_trinh_kiểu_slide_screenshot_2026_11_24_133754_source_footer [INFERRED 0.85]
- **KPI slide layout pattern: takeaway title, hero number, breakdown, unit chart, sourced footnote** — slide_thuyet_trinh_kiểu_slide_screenshot_2026_11_24_133808_headline_takeaway_title, slide_thuyet_trinh_kiểu_slide_screenshot_2026_11_24_133808_hero_kpi_number, slide_thuyet_trinh_kiểu_slide_screenshot_2026_11_24_133808_kpi_breakdown_row, slide_thuyet_trinh_kiểu_slide_screenshot_2026_11_24_133808_unit_waffle_chart, slide_thuyet_trinh_kiểu_slide_screenshot_2026_11_24_133808_source_footnote [INFERRED 0.85]

## Communities (46 total, 3 thin omitted)

### Community 0 - "Project Docs & Pipeline Overview"
Cohesion: 0.06
Nodes (63): BAO_CAO_DU_LIEU.md - Báo cáo thu thập và làm sạch dữ liệu, Hạn chế dữ liệu và crawler chưa sửa, Bảng lương trung vị theo cấp bậc (767 vs 357 tin), CLAUDE.md - project guide (KDD pipeline, progress, checklist), clean_data.csv (3.204 × 41), clean_data_processed.csv (3.135 × 47, is_primary), cluster_assignments.csv (kết quả cụm khóa), Danh sách lỗi crawler K1–K16 (+55 more)

### Community 1 - "Step Mini-Notes & Shared Sample"
Cohesion: 0.06
Nodes (55): MiniNote 2.1 Cleaning, cities_clean / n_cities / work_mode_clean, clean_data.csv (3,204 rows), clean_data_processed.csv (3,135 x 47), description_clean and text_for_tfidf (stop words removed), experience_group and seniority_clean (Intern split, multi-level titles), Duplicate groups: keep one primary record (3,204 -> 3,135), Salary checks, period-assumption flag and analysis eligibility (+47 more)

### Community 2 - "Slide Outline (18 slides)"
Cohesion: 0.05
Nodes (57): Apriori and FP-Growth cross-check (100% identical itemsets), ARI stability across seeds (20 seeds, median 96,3% labels kept), Audit: 100% rows have URL_Goc + Timestamp (17/11/2026 17:37:59 → 22:07:29), Backup slides D1–D5 (live demo, column dictionary, hand-computed rule, normalized confusion, source mix), Cluster: Backend Java (331, Spring Boot 5,1×), Cluster: Data Science / ML / LLM (296), Cluster: DevOps / Cloud (323, Ansible 5,4×, Terraform 5,2×), Cluster: Dữ liệu doanh nghiệp SQL/BI/ERP (339) (+49 more)

### Community 3 - "Listing Crawlers"
Cohesion: 0.07
Nodes (21): interactive_scrape(), timestamp(), browser(), persistent_context(), webkit_context(), fetch_page(), main(), parse_card() (+13 more)

### Community 4 - "Assignment Brief Requirements"
Cohesion: 0.06
Nodes (42): Bai tap lon ITEC3417: Thu thap va khai pha du lieu tuyen dung CNTT Viet Nam, NhomXX_BaoCao_BaiTapLon.pdf, Buoc 2.1: Tien xu ly & Lam sach du lieu (Data Cleaning), Buoc 2.2: Phan tich kham pha & Thong ke mo ta (EDA), Buoc 2.3: Gom cum phan khuc thi truong (K-Means / Hierarchical), Buoc 2.4: Khai pha luat ket hop ky nang (Apriori / FP-Growth), Buoc 2.5: Phan lop / Du bao (khoang luong <15M,15-30M,>30M hoac cap bac), Cau truc bao cao tong ket PDF (6 chuong, 15-25 trang) (+34 more)

### Community 5 - "Report PDF Findings"
Cohesion: 0.05
Nodes (39): Chống chặn: random.uniform delay, ẩn navigator.webdriver, restart khi Cloudflare, Apriori vs FP-Growth: cùng 856 tập phổ biến, Luật kết hợp buoc2.4 (mlxtend, 2.430 giỏ × 121), Audit: 100% URL_Goc + Timestamp, clean_data.csv (3.204 × 41), 8 cụm: Frontend/Web, Dữ liệu doanh nghiệp, Backend Java, DevOps/Cloud, Data Science/ML/LLM, Mạng & bảo mật, Agile/Scrum, C/C++, Độ bền cụm: 20/20 seed, trung vị 96,3% giữ nhãn, Silhouette 0,074, Chương 6: tri thức, khuyến nghị, hạn chế (+31 more)

### Community 6 - "Detail Fetch Extractors"
Cohesion: 0.10
Nodes (21): CloudflareDetected, description_complete(), extract_generic(), extract_job_posting(), extract_topcv(), extract_topdev(), extract_vietnamworks(), fetch_linkedin() (+13 more)

### Community 7 - "JSONL I/O & IT Filter"
Cohesion: 0.15
Nodes (11): read_jsonl(), write_jsonl(), main(), extract_row(), main(), skills_in_jd(), classify_row(), load_dictionary() (+3 more)

### Community 8 - "Field Normalization (lam_sach)"
Cohesion: 0.17
Nodes (14): clean_record(), normalize(), benefits(), clean_company(), _exp_from_text(), is_intern(), is_pm_po(), jd_sections() (+6 more)

### Community 9 - "EDA Skill Charts"
Cohesion: 0.11
Nodes (20): buoc2.2_EDA.ipynb, Chart: Co cau mau theo nguon thu thap (source composition bar chart, step 2.2 EDA), Sample source imbalance: TopCV dominant (~1,140), LinkedIn ~640, ITviec ~550, VietnamWorks ~480, TopDev ~60, TopCV source (largest share of EDA sample), TopDev source (smallest share, ~60 postings), Heatmap: skill mention rate by seniority, Cloud/DevOps group (Junior n=49, Middle n=37, Senior n=58), DevOps and CI/CD are core at every level (DevOps 78-93%, CI/CD 69-81%, highest at Senior), Junior postings stress hands-on tools: Docker 65.31%, Linux 63.27%, Bash 59.18%, GitLab/Jenkins 46.94%, all lower at Senior (+12 more)

### Community 10 - "Report: Funnel & Salary Handling"
Cohesion: 0.18
Nodes (17): Báo cáo Bài tập lớn Nhóm 4 (Nhom04_BaoCao_BaiTapLon.md), Câu hỏi nghiên cứu Q1–Q5, Chuẩn hóa lương bằng Regex về VND/tháng (1 USD = 25.000), Cơ cấu nguồn và độ bao phủ lương (Bảng 3.1; TopCV 68,02% tập lương), Ghép tin trùng chéo nguồn (group_jobs, rapidfuzz ≥ 90), Hạn chế toàn quy trình (một đợt cào, lương thiên TopCV, 266 pending_review), Hướng phát triển (đọc tay pending_review, chạy lại phễu 7.308, thêm đặc trưng, cào theo tháng), Khóa định danh: job_uid, URL_Goc, job_group_id + is_primary (+9 more)

### Community 11 - "Association Rules (2.4)"
Cohesion: 0.18
Nodes (20): Apriori (856 tập phổ biến, 0,102 s), Cụm DevOps / Cloud (323), Độ nhạy luật: mẫu số 2.843 tin (1.225/1.228) và MIN_COUNT, FP-Growth (856 tập, 0,279 s), Giỏ kỹ năng 2.430 × 121 (MIN_COUNT = 30, ≥ 2 kỹ năng), Luật android ⇄ ios (lift ≈ 16), Luật kubernetes ⇒ docker (73,6%, lift 3,33; tính tay 4.2.6), Luật python ⇒ sql liên kết yếu (lift 1,14) (+12 more)

### Community 12 - "K-Means Clustering (2.3)"
Cohesion: 0.16
Nodes (15): cluster_assignments.csv (2.843 dòng), Cụm Quy trình Agile / Scrum (206), Cụm Backend Java (331), Cụm C / C++ và Linux (149), Cụm Data Science / ML / LLM (296), Cụm Dữ liệu doanh nghiệp SQL/BI/ERP (339), Cụm Frontend và Web (363), Cụm Hệ thống, mạng và bảo mật (211) (+7 more)

### Community 13 - "CSV Export"
Cohesion: 0.17
Nodes (9): cell(), final_it_decision(), job_uid(), main(), raw_record(), read_review(), review_rows(), write_csv() (+1 more)

### Community 14 - "Lift Heatmaps"
Cohesion: 0.16
Nodes (19): Heatmap: Lift giữa các cặp kỹ năng phổ biến (step 2.4), aws - azure (lift 3.2), Backend skill block (java, microservices, postgresql, rest api, git; lift 1.8-2.7), DevOps/Cloud skill block (aws, azure, ci/cd, devops, docker, kubernetes; lift 1.9-3.3), devops - kubernetes (lift 2.9), docker - kubernetes (lift 3.3, highest), Lift (association rule metric), microservices - rest api (lift 2.7) (+11 more)

### Community 15 - "Salary Classification (2.5)"
Cohesion: 0.18
Nodes (18): Bài toán phân lớp khoảng lương (<15, 15–30, >30 triệu; 688 tin), Biên bản phân công nhiệm vụ (WBS, 25% mỗi người), Chuẩn hóa địa điểm, kinh nghiệm, cấp bậc kèm *_source (lam_sach.py), Phân lớp khoảng lương (< 15, 15-30, > 30 triệu; 688 tin), Ma trận nhầm lẫn RF (83/138 đúng, lớp 15-30 recall 47,7%), Decision Tree cắt tỉa ccp_alpha (0,01; sâu 3, 6 lá; test F1 0,5758), DummyClassifier mốc (Macro F1 0,2135), Phân tích 5 tin dự đoán sai (Peru USD, 10 năm 12,5M...) (+10 more)

### Community 16 - "Tuấn's 2.1/2.2 Slides"
Cohesion: 0.23
Nodes (14): clean_data_processed.csv (3.135 × 47), Đối chiếu theo tên công ty và nguồn (độ bền top kỹ năng), Kỹ năng theo cấp bậc trong 4 nhóm nghề (heatmap, ngưỡng ≥ 20 tin), Ma trận nhị phân kỹ năng 2.843 × 157, Nguyễn Lê Anh Tuấn (notebook 2.1, EDA; mục 2.8, Ch.3), So sánh vùng HN/TP.HCM/Đà Nẵng (trọng số 1/k), Top 15 kỹ năng (Python 32,08%, SQL 28,21%, CI/CD 25,01%), Nội dung slide Bước 2.1 và 2.2 (Tuấn) (+6 more)

### Community 17 - "Salary Parsing"
Cohesion: 0.28
Nodes (5): clean_row(), jd_amount(), monthly_ranges(), normalize(), parse_salary()

### Community 18 - "Cross-Source Job Grouping"
Cohesion: 0.25
Nodes (5): assign_groups(), norm_company(), norm_title(), pair_scores(), primary_key()

### Community 19 - "PCA Cluster Chart (slides 2.1/2.2)"
Cohesion: 0.36
Nodes (11): Cluster 7: Quy trình Agile / Scrum, Cluster 3: Backend Java, Cluster 8: C / C++ và Linux, PCA 2D small-multiples chart of 8 K-Means skill clusters (step 2.3), Cluster 5: Data Science / ML / LLM, Cluster 4: DevOps / Cloud, Cluster 2: Dữ liệu doanh nghiệp, Cluster 1: Frontend và Web (+3 more)

### Community 20 - "PCA Cluster Chart (slides 2.3)"
Cohesion: 0.38
Nodes (11): Cluster 7: Quy trình Agile / Scrum, Cluster 3: Backend Java, Cluster 8: C / C++ và Linux, Cluster 5: Data Science / ML / LLM, Cluster 4: DevOps / Cloud, Cluster 2: Dữ liệu doanh nghiệp, Cluster 1: Frontend và Web, Cluster 6: Hệ thống, mạng và bảo mật (+3 more)

### Community 21 - "Đức's Crawler/Pipeline Slides"
Cohesion: 0.27
Nodes (8): Crawler theo nguồn (Bảng 2.1: ITviec, TopDev, VietnamWorks API, TopCV/LinkedIn bán tự động), Thách thức kỹ thuật: tải động, chống chặn, lương ẩn JSON-LD, checkpoint, Trích xuất kỹ năng và bẫy so khớp (React Native ≠ React, C/R/Go), Từ điển kỹ năng 161 mục (135 tech + 26 keywords), Vũ Tiến Đức (crawler, pipeline, gom cụm; Tóm tắt, Ch.1, 2.1–2.7, 4.1), Ý tưởng slide phần Đức (Crawler + Pipeline, Bước 2.3), Phần A: Crawler và Pipeline (slide A1–A5), Việc cần làm trước khi dựng slide (ảnh chụp, tránh đường dẫn /Users/tuan, sửa slide 3 SuonSlide)

### Community 22 - "IT General Skill Heatmap"
Cohesion: 0.24
Nodes (10): Heatmap: skill mention rate by seniority (IT tong quat), Cloud/DevOps skills rise with seniority: AWS 22.00% Senior, Azure 21.67% Lead, CI/CD 18.00% Senior, C# more common at Intern/Junior (12.00%/11.95%) than Lead (3.33%), Cybersecurity 23.33% and Firewall 18.33% at Lead, ERP highest at Lead (28.33%), LLM mention rises from 0-5% (Intern-Junior) to 12-15% (Middle-Lead), Python peaks at Intern (32.00%), Fresher only 6.25%, Seniority levels Intern/Fresher/Junior/Middle/Senior/Lead (n=25/32/159/82/100/60) (+2 more)

### Community 23 - "Cluster Size Chart (slides 2.1/2.2)"
Cohesion: 0.42
Nodes (10): Chart: Số tin của từng cụm (cluster sizes, K=8, 2,218 jobs), Cluster: Quy trình Agile / Scrum - 206 (9%), Cluster: Backend Java (Spring, Microservices) - 331 (15%), Cluster: C / C++ và Linux - 149 (7%), Cluster: Data Science / ML / LLM - 296 (13%), Cluster: DevOps / Cloud (CI/CD, Terraform, K8s) - 323 (15%), Cluster: Dữ liệu doanh nghiệp (SQL, BI, ERP) - 339 (15%), Cluster: Frontend và Web (HTML, JS, React) - 363 (16%) (+2 more)

### Community 24 - "Cluster Size Chart (slides 2.3)"
Cohesion: 0.38
Nodes (10): Chart: Số tin của từng cụm (cluster sizes, K=8), Cluster: Quy trình Agile / Scrum - 206 (9%), Cluster: Backend Java (Spring, Microservices) - 331 (15%), Cluster: C / C++ và Linux - 149 (7%), Cluster: Data Science / ML / LLM - 296 (13%), Cluster: DevOps / Cloud (CI/CD, Terraform, K8s) - 323 (15%), Cluster: Dữ liệu doanh nghiệp (SQL, BI, ERP) - 339 (15%), Cluster: Frontend và Web (HTML, JS, React) - 363 (16%) (+2 more)

### Community 25 - "Pruned Tree Figure (output)"
Cohesion: 0.28
Nodes (8): Pruned Decision Tree Figure (top levels), Balanced class weights (root value 183.333 each over 550 samples), Salary class 15_den_30 (15-30M VND), Salary class duoi_15 (< 15M VND), Salary class tren_30 (> 30M VND), Pruned Decision Tree (salary band, depth 3, 6 leaves), Split feature: experience__exp_min_years (<=1.5 root, <=0.5, <=3.5), Split feature: experience__missingindicator_exp_max_years (<=0.5)

### Community 26 - "Intra-Site Dedup"
Cohesion: 0.36
Nodes (5): city_key(), key_of(), main(), norm(), richer()

### Community 27 - "Rules Scatter (output)"
Cohesion: 0.33
Nodes (7): Scatter: Support vs Confidence of non-redundant association rules (color = Lift), Confidence threshold 0.5, Insight: highest-lift rules have low support (0.03-0.07), Lift color scale (about 1.2 to 16, viridis), Non-redundant association rules (đã loại dư thừa), Rules with confidence 1.0 (conviction infinite), Support threshold 0.03

### Community 28 - "Style Ref: KPI Cards"
Cohesion: 0.43
Nodes (7): Slide style reference: KPI-card financial slide (Fable 5.2 output), Kicker + takeaway headline title pattern, Restrained navy/blue/grey/orange palette, Source citation footer, Single stacked bar with value legend, Three-column KPI card layout, Waterfall chart (cash flow bridge)

### Community 31 - "Socratic Tutor Skill"
Cohesion: 0.47
Nodes (3): Socratic Tutor skill (SKILL.md), Diagnostic placement (3-5 questions), Skill map levels 0-5

### Community 32 - "Pruning Curve (output)"
Cohesion: 0.53
Nodes (6): Pruning curve: ccp_alpha vs macro F1 (train/validation), ccp_alpha cost-complexity pruning, Decision Tree salary band classifier, Selected alpha = 0.0081 (max validation macro F1 ~0.63), 3-fold CV mean (train and validation), Train-validation macro F1 gap (overfitting)

### Community 33 - "Data/AI Skill Heatmap"
Cohesion: 0.40
Nodes (6): Heatmap: skill mention rate by seniority, Data/AI role group (EDA 2.2), Data/AI role group, Python and SQL as core Data/AI skills across all levels, Senior Data/AI postings emphasise ML, cloud (AWS/Azure), CI/CD, Docker, Spark, Seniority levels Junior/Middle/Senior/Lead, Skill share by seniority within role group (Data/AI)

### Community 34 - "Salary by Level Boxplot"
Cohesion: 0.47
Nodes (6): Boxplot: Trung diem khoang luong theo cap bac (2.2), EDA 2.2: salary by level, Lead median salary below Senior, Salary midpoint (Luong_TB, trieu VND), Salary outliers (Lead ~187.5M, Senior ~112M), Seniority levels (Intern/Fresher/Junior/Middle/Senior/Lead/Multi-level/Unknown)

### Community 35 - "Rules Scatter (slides)"
Cohesion: 0.40
Nodes (6): Scatter: Support vs Confidence of non-redundant association rules (color = Lift), Confidence threshold 0.5 (y-axis 0.5-1.0), Insight: high lift only at low support; confidence=1.0 rules cluster at support 0.03-0.05, Lift color scale (~1.4 to 16, viridis), Non-redundant association rules (skill baskets), Support threshold 0.03 (x-axis starts at 0.03, max ~0.157)

### Community 36 - "Pruned Tree Figure (slides)"
Cohesion: 0.47
Nodes (6): Balanced class weights (value = 183.333 each at root, 550 samples), Gini impurity split criterion (root gini 0.667), Pruned Decision Tree (salary band, depth 3, 6 leaves), Salary band classes: duoi_15 / 15_den_30 / tren_30, Split feature: experience__exp_min_years (thresholds 1.5 / 0.5 / 3.5), Split feature: experience__missingindicator_exp_max_years

### Community 37 - "Pruning Curve (slides)"
Cohesion: 0.53
Nodes (6): ccp_alpha cost-complexity pruning, Decision Tree salary-band classifier (step 2.5), Macro F1 (mean of 3 CV folds), Overfitting gap (train vs validation macro F1), Decision Tree ccp_alpha vs macro F1 (train/validation) chart, Chosen alpha = 0.0081

### Community 38 - "RF Confusion Counts (output)"
Cohesion: 0.60
Nodes (5): Confusion matrix (counts) - RandomForest salary band, test set, Middle band 15_den_30 most confused, RandomForest salary-band classifier, Salary bands duoi_15 / 15_den_30 / tren_30, Test set of 138 jobs

### Community 39 - "Elbow/Silhouette (slides 2.3)"
Cohesion: 0.60
Nodes (5): Elbow: inertia theo K, Elbow + Silhouette chart for choosing K (step 2.3 clustering slide), Chosen K = 8, K passing selection rule (K = 2, 3, 6, 7, 8), Silhouette theo K

### Community 40 - "RF Confusion Counts (slides)"
Cohesion: 0.60
Nodes (3): Random Forest salary-band classifier (step 2.5 comparator), RandomForest confusion matrix (counts, salary band test set n=138; correct 20+31+32=83, accuracy ~0.601), Salary band labels duoi_15 / 15_den_30 / tren_30 (VND million/month)

### Community 41 - "RF Normalized Confusion"
Cohesion: 0.67
Nodes (4): RandomForest normalized confusion matrix (salary band), Middle band 15_den_30 lowest recall (0.48), RandomForest salary-band classifier, Salary band classes duoi_15 / 15_den_30 / tren_30

### Community 42 - "RF Recall by Band"
Cohesion: 0.67
Nodes (4): Recall by salary band chart (RandomForest), Low recall on middle salary band 15_den_30, RandomForest salary-band classifier, Salary bands duoi_15 / 15_den_30 / tren_30

### Community 43 - "Region Comparison Chart"
Cohesion: 0.67
Nodes (3): EDA 2.2 region comparison chart (postings and median salary by city), Median salary midpoint by city, Job postings count by city (Ha Noi, TP.HCM, Da Nang)

### Community 44 - "Elbow/Silhouette (slides 2.1/2.2)"
Cohesion: 0.83
Nodes (3): Chart 2.3: Elbow + Silhouette theo K (K-Means gom cụm kỹ năng), Elbow: inertia theo K (không có khuỷu rõ), Silhouette theo K (0.044-0.082)

## Ambiguous Edges - Review These
- `GIAI_TRINH_KEY.md - giải trình lựa chọn key` → `primary_key: có JD → có Luong_TB → nhiều kỹ năng → JD dài → SOURCE_RANK → job_uid`  [AMBIGUOUS]
  GIAI_TRINH_KEY.md · relation: conceptually_related_to
- `cluster_assignments.csv (kết quả cụm khóa)` → `Bước 2.4 luật kết hợp (Apriori/FP-Growth, 2.430 × 121, 1.228 luật → 1.027 không dư thừa)`  [AMBIGUOUS]
  CLAUDE.md · relation: conceptually_related_to
- `crawler/vietnamnetworks.py - gọi Search API ms.vietnamworks.com bằng requests` → `Key cấu hình request (User-Agent, Referer, hitsPerPage, filter.jobFunction, retrieveFields, keywords)`  [AMBIGUOUS]
  GIAI_TRINH_KEY.md · relation: conceptually_related_to
- `Bao cao buoc 2.4 Luat ket hop` → `data/cluster_assignments.csv`  [AMBIGUOUS]
  buoc 2.3/miniNote_buoc2.3_GomCum.md · relation: references
- `Báo cáo Bài tập lớn Nhóm 4 (Nhom04_BaoCao_BaiTapLon.md)` → `Ý tưởng slide phần Đức (Crawler + Pipeline, Bước 2.3)`  [AMBIGUOUS]
  slide thuyet trinh/Nhom04_YTuongSlide_Duc.md · relation: cites
- `Tập IT dùng sơ bộ (2.869 tin, 2.843 có JD; 266 pending_review)` → `Độ tin cậy, hạn chế và kết luận bước 2.4`  [AMBIGUOUS]
  slide thuyet trinh/slide_thuyet_trinh_buoc_2.4.md · relation: conceptually_related_to
- `cluster_assignments.csv (2.843 dòng)` → `Luật kết hợp buoc2.4 (mlxtend, 2.430 giỏ × 121)`  [AMBIGUOUS]
  file báo cáo bài nhóm/Nhom04_BaoCao_BaiTapLon.pdf · relation: conceptually_related_to
- `Lift (association rule metric)` → `Blank off-diagonal cells (agile-linux, java-linux, linux-microservices: pair not among frequent itemsets)`  [AMBIGUOUS]
  buoc2.4/output/heatmap_lift.png · relation: conceptually_related_to

## Knowledge Gaps
- **91 isolated node(s):** `README.md (placeholder, 2 dòng)`, `PCA 2D small-multiples plot (~13% variance, illustration only)`, `Source-skewed clusters (network/security TopCV 57%, C/C++ LinkedIn 48%)`, `Top 15 skills (denominator = jobs with JD)`, `Salary by seniority (median, IQR, boxplot; non-assumed-period subset)` (+86 more)
  These have ≤1 connection - possible missing edges or undocumented components. (Counts symbols only; 186 node(s) total have ≤1 connection when file, concept and rationale nodes are included.)
- **3 thin communities (<3 nodes) omitted from report** — run `graphify query` to explore isolated nodes.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `Báo cáo Bài tập lớn Nhóm 4 (Nhom04_BaoCao_BaiTapLon.md)` connect `Report: Funnel & Salary Handling` to `Association Rules (2.4)`, `K-Means Clustering (2.3)`, `Salary Classification (2.5)`, `Tuấn's 2.1/2.2 Slides`, `Đức's Crawler/Pipeline Slides`?**
  _High betweenness centrality (0.126) - this node is a cross-community bridge._
- **What connects `README.md (placeholder, 2 dòng)`, `PCA 2D small-multiples plot (~13% variance, illustration only)`, `Source-skewed clusters (network/security TopCV 57%, C/C++ LinkedIn 48%)` to the rest of the system?**
  _91 weakly-connected nodes found - possible documentation gaps or missing edges._
- **Should `Project Docs & Pipeline Overview` be split into smaller, more focused modules?**
  _Cohesion score 0.055176890619928594 - nodes in this community are weakly interconnected._
- **Why does `clean_data_processed.csv (3.135 × 47)` connect `Tuấn's 2.1/2.2 Slides` to `Report: Funnel & Salary Handling`, `K-Means Clustering (2.3)`, `Report PDF Findings`, `Salary Classification (2.5)`?**
  _High betweenness centrality (0.039) - this node is a cross-community bridge._
- **Should `Step Mini-Notes & Shared Sample` be split into smaller, more focused modules?**
  _Cohesion score 0.0633879781420765 - nodes in this community are weakly interconnected._

### Low-confidence Hints
_AMBIGUOUS edges — the extractor was unsure. Verify before acting on these._

- **What is the exact relationship between `GIAI_TRINH_KEY.md - giải trình lựa chọn key` and `primary_key: có JD → có Luong_TB → nhiều kỹ năng → JD dài → SOURCE_RANK → job_uid`?**
  _Edge tagged AMBIGUOUS (relation: conceptually_related_to) - confidence is low._
- **What is the exact relationship between `cluster_assignments.csv (kết quả cụm khóa)` and `Bước 2.4 luật kết hợp (Apriori/FP-Growth, 2.430 × 121, 1.228 luật → 1.027 không dư thừa)`?**
  _Edge tagged AMBIGUOUS (relation: conceptually_related_to) - confidence is low._