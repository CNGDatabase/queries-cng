# queries-cng


## 1. Check for Weird Published Dates
This query shows there are unexpected published dates for a particular entity:

```sql
SELECT pm.*, p.post_date, p.post_status, p.post_type
FROM natural_postmeta pm
INNER JOIN natural_posts p ON p.ID = pm.post_id
WHERE pm.meta_key = 'inspected_farm'
  AND meta_value = 'Adams Home Grown Produce'
ORDER BY pm.post_id ASC;
## 2. Applications Missing a Corresponding Entity
This query identifies applications without a corresponding entity:

```sql
SELECT p.post_author, COUNT(*) AS missing
FROM natural_posts p
LEFT JOIN natural_natg_entities e ON e.user_id = p.post_author
WHERE p.post_type = 'application'
  AND e.id IS NULL
GROUP BY p.post_author;
