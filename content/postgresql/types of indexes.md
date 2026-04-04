B+
- 
hash
- uses a hash function to map values in a column to the corresponding rows in a table. 
- the hash function takes a value from the indexed column and computes a hash value --> which is then used 
```
SELECT * FROM library WHERE isbn = '978-3-16-148410-0';
```

bitmap 


B+ vs hash vs bitmap 

- space efficiency: 

|        | advantages                                                     | disadvantage |
| ------ | -------------------------------------------------------------- | ------------ |
| bitmap | for queries that involve conditions on low-cardinality columns |              |


clustered index --> how data is physically stored in the memory 
tables' rows stored in the same order as clustered index
non-clustered index is a separate structure that contains a copy of the indexed columns and a pointer to the corresponding rows in the table. 

1 table - 1 clustered index (faster for range queries & sorting operations)
1 table - multiple non-clustered indexes

---

flight table has 500 million rows
multilevel index is built on flight_id 


```sql
SELECT * FROM flights
WHERE airline = 'Delta'
ORDER BY departure_time ASC;
-- airline has 12 distinct values
```

db administrator adds a bitmap index on flght_id to speed up lookups 



???????????what is high cardinality column?
