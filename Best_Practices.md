### Best Practices
Use a meaningful name for constraints (e.g., u_species for "unique species").  
Follow a consistent naming convention, such as:
- **u_columnName** (for unique constraints)
- **pk_tableName** (for primary keys)
- **fk_table1_table2** (for foreign keys)


The best practice depends on your team’s standards:  
FK_<child_table>_<parent_table> → FK_Pets_People (short and clear)  

FK_<child_table>_<child_column>_<parent_table>_<parent_column> → FK_Pets_OwnerID_People_ID (more detailed)  