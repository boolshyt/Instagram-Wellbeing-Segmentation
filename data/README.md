# Data dictionary

The two CSVs used in this project are about **55 MB each** and are therefore not stored in the repository. Place them in this folder (or next to the notebook) to run the analysis locally.

## Source

**Kaggle:** [Social Media User Analysis](https://www.kaggle.com/datasets/rockyt07/social-media-user-analysis)
Dataset provided by Epitech for the Rush 4 case study. The data is **synthetic** (artificially generated); the project includes a formal demonstration of this (section 6 of the main README, section 7 of the technical report).

## Files

| File | Rows | Columns | Role in the project |
|---|---|---|---|
| `instagram_usage_lifestyleSafe.csv` | 200,000 | 58 | **Working file** used for the whole analysis |
| `instagram_users_lifestyleSafe.csv` | 200,000 | 58 | Kept for the consistency audit only |

The two files share an identical schema. A line-by-line comparison (notebook, section 1.1) shows 52 of 58 columns are strictly identical, including every usage and well-being variable. Only 6 secondary profile fields diverge: `employment_status`, `education_level`, `relationship_status`, `has_children`, `weekly_work_hours`, `account_creation_year`.

## Variables (58 columns, one row = one user)

### Identifiers
| Column | Type | Notes |
|---|---|---|
| `user_id` | int | unique, 1 to 200,000 |
| `app_name` | text | constant ("Instagram") |

### Demographics and profile
`age` (13-65, includes 18,913 minors), `gender`, `country` (10 values), `urban_rural`, `income_level`, `employment_status`, `education_level`, `relationship_status`, `has_children`, `account_creation_year`

### Instagram usage
| Group | Columns |
|---|---|
| Intensity | `daily_active_minutes_instagram` (5-565), `sessions_per_day`, `average_session_length_minutes`, `last_login_date` |
| Time per surface | `time_on_feed_per_day`, `time_on_reels_per_day`, `time_on_explore_per_day`, `time_on_messages_per_day` |
| Content activity | `posts_created_per_week`, `reels_watched_per_day`, `stories_viewed_per_day` |
| Social interactions | `likes_given_per_day`, `comments_written_per_day`, `dms_sent_per_week`, `dms_received_per_week` |
| Network | `followers_count`, `following_count` |
| Ads and monetization | `ads_viewed_per_day`, `ads_clicked_per_day`, `subscription_status`, `uses_premium_features` |
| Settings | `privacy_setting_level`, `two_factor_auth_enabled`, `biometric_login_used`, `linked_accounts_count`, `notification_response_rate`, `content_type_preference`, `preferred_content_theme` |
| Composite | `user_engagement_score` (0-100; **excluded from modeling**: it correlates at -0.53 with actual usage minutes, an unexplained contradiction) |

### Well-being (the three outcome variables)
| Column | Scale | Note |
|---|---|---|
| `perceived_stress_score` | **0-40** | the accompanying documentation wrongly states 0-10; the data was verified and the 0-40 scale confirmed |
| `sleep_hours_per_night` | 3-10 h | concentrated around 7 h |
| `self_reported_happiness` | 1-10 | |

All three are self-reported (declared by the user), which is a documented limitation.

### Lifestyle and health
`exercise_hours_per_week`, `diet_quality`, `smoking`, `alcohol_frequency`, `body_mass_index`, `blood_pressure_systolic`, `blood_pressure_diastolic`, `daily_steps_count`, `weekly_work_hours`, `hobbies_count`, `social_events_per_month`, `books_read_per_year`, `volunteer_hours_per_month`, `travel_frequency_per_year`

In this dataset, lifestyle variables carry **zero signal** about well-being (R² = 0.000 for predicting stress), one of the fingerprints showing the data is synthetic.

## Quality summary

| Check | Result |
|---|---|
| Missing values | 0 |
| Duplicate rows / duplicate user_id | 0 / 0 |
| Out-of-range values (after correcting the stress scale) | 0 |
| Documented anomalies | stress scale mislabeled in docs; `user_engagement_score` contradicts usage; two near-identical source files |
