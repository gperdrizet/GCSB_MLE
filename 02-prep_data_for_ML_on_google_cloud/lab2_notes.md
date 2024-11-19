# Lab 2: Dataprep: Qwik Start

## Objectives

- Import data
- Correct mismatched data
- Transform data
- Join data

## 1. Create a Cloud Storage bucket in your project

- Navigation menu > Cloud Storage > Buckets > Create bucket

Give the new bucket a unique name & uncheck 'Enforce public access prevention on this bucket', then click *Create*.

**Name**: gperdrizet_lab2

## 2. Initialize Cloud Dataprep

From Cloud Shell:

```bash
gcloud beta services identity create --service=dataprep.googleapis.com
```

Then,

- Navigation menu > Dataprep
- Accept terms
- Agree to account info. sharing
- Click student username to sign into cloud dataprep by Trifacta
- Click allow, click agree
- Click continue from 'First time setup'

## 3. Create a flow

- Flows > Create > Blank Flow
- Untitled Flow > name: FEC-2016, description: United States Federal Elections Commission 2016

## 4. Import datasets

- Add Datasets > Import Datasets
- Cloud storage > edit

Paste `gs://spls/gsp105` in 'Choose a file or folder', click go.

- us-fec/ > + next to cn-2016.txt > name: Candidate Master 2016
- itcont-2016-orig.txt > name: Campaign Contributions 2016

Click 'Import & Add to Flow' under the datasets on the right.

## 5. Prep the candidate file

- Click 'Edit Recipe` on the Candidate Master 2016 dataset.
- Click tallest bin in column 5 histogram (year)
- In Suggestions, Keep rows > Add

- Click red highlighted portion of column 6 (state)
- Close the suggested transformation
- Click the flag icon on column 6 and set datatype string

- Click the 'P' bin in column 7
- Add the suggestion

## 6. Wrangle the Contributions file and join it to the Candidates file

- Click FEC-2016 dataset via the selector drop-down at the top left
- Select 'Campaign Contributions 2016'
- Add > Recipe > Edit Recipe
- Click Recipe icon > add new step

Paste the following into the search box:

```text
replacepatterns col: * with: '' on: `{start}"|"{end}` global: true
```

- Click 'Add', then 'New Step' again and search for 'Join', select 'Join datasets'. Then select 'Candidate Master 2016'.
- Click pencil icon in 'join keys'.
- Select column2 = column11 from suggested join keys, then save and continue.
- Click 'Next', then select all columns.
- 'Review' then 'Add to Recipe'.

## 7. Summary of data

- From Recipe panel, click 'New Step'
- Enter the following in the 'Transformation' search box

```text
pivot value:sum(column16),average(column16),countif(column16 > 0) group: column2,column24,column8
```

Click 'Add'

## 8. Rename columns

- New Step, then paste:

```text
rename type: manual mapping: [column24,'Candidate_Name'], [column2,'Candidate_ID'],[column8,'Party_Affiliation'], [sum_column16,'Total_Contribution_Sum'], [average_column16,'Average_Contribution_Sum'], [countif,'Number_of_Contributions']
```

- Click 'Add'
- Add one more step to round avg. contribution amount:

```text
set col: Average_Contribution_Sum value: round(Average_Contribution_Sum)
```

Done!
