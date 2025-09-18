# CNG Database Queries

## 1. Check for Weird Published Dates
```sql
SELECT pm.*, p.post_date, p.post_status, p.post_type
FROM natural_postmeta pm
INNER JOIN natural_posts p ON p.ID = pm.post_id
WHERE pm.meta_key = 'inspected_farm'
  AND meta_value = 'Adams Home Grown Produce'
ORDER BY pm.post_id ASC;
```

## 2. Applications Missing a Corresponding Entity
```sql
SELECT p.post_author, COUNT(*) AS missing
FROM natural_posts p
LEFT JOIN natural_natg_entities e ON e.user_id = p.post_author
WHERE p.post_type = 'application'
  AND e.id IS NULL
GROUP BY p.post_author;
```

### Adjusted for Original Relationship
```sql
SELECT p.post_author, COUNT(*) AS missing
FROM natural_posts p
LEFT JOIN (
    SELECT DISTINCT post_author
    FROM natural_posts
    WHERE post_type = 'cng_farm'
) farms ON farms.post_author = p.post_author
WHERE p.post_type = 'application'
  AND farms.post_author IS NULL
GROUP BY p.post_author;
```

## 3. Orphaned Data
```sql
SELECT DISTINCT pm.meta_value
FROM natural_postmeta pm
LEFT JOIN natural_natg_entities e
       ON e.entity_name = pm.meta_value
WHERE pm.meta_key = 'inspected_farm'
  AND e.id IS NULL;
```

### Adjusted for Your Environment
```sql
SELECT DISTINCT pm.meta_value
FROM natural_postmeta pm
LEFT JOIN natural_posts f
       ON f.post_title = pm.meta_value
      AND f.post_type = 'cng_farm'
WHERE pm.meta_key = 'inspected_farm'
  AND f.ID IS NULL;
```
