**Fix2Fix**

## Data files

### `FIV_1195_Records.xlsx` — all 1,195 candidate records

Every candidate pair in the dataset: a vulnerability-fixing commit (the previous fix) and a later commit that changes the same or nearby code in the same file. Candidates are linked by code proximity, so a record is a candidate, not a confirmed fix-induced vulnerability.

One sheet, 1,195 rows, 10 columns: Project, commit (the previous fix), Lines Added in Fixed Version, Lines Deleted in Fixed Version, Hunks in Fixed Version, codeLink (GitHub link to the fix), CWE ID, CVE ID (present for 1,062 rows), vulnerability_fixed_again_in_version (the file version in which the later commit appears), Prompt File (the prompt built from the pair).

### `FIV_195_Instances.xlsx` — the final 195 fix-induced vulnerabilities

The subset of the 1,195 records confirmed as fix-induced vulnerabilities (FIVs): the later commit repairs a new weakness that the previous fix introduced. 217 records were judged FIV by two independent annotators; 22 were excluded on review because the weakness predated the fix or the commit pairing was broken, leaving 195.

- Sheet `FIV Instances`: 195 rows. The same 10 columns as above, plus Category, Insight Number and Insight.
- Sheet `Subtotals`: counts per insight and per category, computed from the first sheet.

146 of the 195 fall into twelve recurring insights in six categories. The other 49 share no recurring mechanism and have the category `No recurring mechanism`.

| Category | Insight | Instances |
|---|---|---|
| Misaligned Validation Checks | 1. Added Check → Wrong Scope Consulted | 11 |
| | 2. Substitute Condition → Legitimate Work Refused | 9 |
| Resource Ownership and Lifetime Violations | 3. Added Early Exit → Skipped Cleanup | 20 |
| | 4. Added Release → Double Free | 14 |
| | 5. Relocated Release → Use After Free | 7 |
| Incorrect Bounds and Capacity Enforcement | 6. Bounds or Predicate Rewrite → Boundary Off-by-One | 28 |
| | 7. Guard Arithmetic → Wrapped Bound | 14 |
| | 8. Derived Bound → Omitted Term | 8 |
| Improper Exceptional-State Handling | 9. Added Operation → Absent Participant | 5 |
| | 10. Anomaly Made Fatal → Denial of Service | 5 |
| Inconsistent Representation Updates | 11. Representation Change → Stale Dependent Expression | 17 |
| Reliance on Invalidated State Assumptions | 12. Check Then Act → State Changed In Between | 8 |
| **Total in the twelve insights** | | **146** |
| No recurring mechanism | | 49 |
| **Total FIV instances** | | **195** |

Records in the two files match on `Prompt File`, which is unique per record. `commit` is not unique: one fix can pair with more than one later commit.

## Source data

Full data can be downloaded here
https://drive.google.com/drive/folders/1kV_KN_iUNk59oMYPrNo7BYe77ROJ83La?usp=sharing

Hierarchical format of the data.

```
|--- Project_Name1/
|   |
|   |--- Project_Name_CommitHash1/
|   |   |
|   |   |--- all_versions/
|   |   |   |--- File_v1_DATE_TIME.txt
|   |   |   |--- File_v2_DATE_TIME.txt
|   |   |   |--- File ....
|   |   |
|   |   |--- commit_messages/
|   |   |   |--- File_v1_DATE_TIME_commit_msg.txt
|   |   |   |--- File_v2_DATE_TIME_commit_msg.txt
|   |   |   |--- File ....
|   |   |
|   |   |--- file_changes_in_versions/
|   |   |   |--- File_changes_v1_v2_DATE_TIME.txt
|   |   |   |--- File_changes_v2_v3_DATE_TIME.txt
|   |   |   |--- File ....
|   |
|   |--- Project_Name_CommitHash2/ ....
|   |
|--- Project_Name2/  ....
```

**Dataset creation code**


The dataset provided is ready to use. Here, the  details are provided about the data collection process. If you just want to use the dataset, this part is not required. 
Data Collection.ipynb file extracts all future versions from GitHub for future analysis. 


Data_Process_FullyCleaned.ipynb file processed the data once the future versions are collected.

Sample prompts for 50 samples are available in Prompt1 and PromptFinal directory.

To collect next vulnerability information from GPT, "GPT_nextVulPrompt-Cleaned.ipynb" is used. You need to add your API key.

https://drive.google.com/drive/folders/1WaeLvuX8o17zxjrLtet11PfZjQcz-8Bf?usp=sharing
