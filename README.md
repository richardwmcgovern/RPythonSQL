# RPythonSQL
Useful snippets in R, Python, and SQL, conversions between them, and instances where using more than one is effective.

# Pyjanitor
https://pyjanitor.readthedocs.io/notebooks/medium_franchise.html
Reminder that R is natively superior to Python for data cleaning. Pyjanitor and pandas-flavor help mitigate this.

# Useful Pandas Snippets
## There's a LOT here!!
https://gist.github.com/fomightez/ef57387b5d23106fabd4e02dab6819b4
https://stackoverflow.com/questions/17071871/select-rows-from-a-dataframe-based-on-values-in-a-column-in-pandas?rq=1

df.query(...) is your friend. And can be chained

An easier replace function?
-# Change all NaNs to None (useful before
-# loading to a db)
df = df.where((pd.notnull(df)), None)

Is this preferable to df.apply? What is pattern and what is anti-pattern?
df = df.applymap(lambda x: str(x).strip() if len(str(x).strip()) else None)

-# string replacement for index strings (hopefully `.replace` gets added Index soon and this becomes moot, but for now:
replace_indx = lambda x,d: d[x] if x in d else x
idx = pd.Index(['a',"b","c"])
idx.map(lambda x:replace_indx(x, {"b":"fIXED_B"}))

Numbers stored as strings? Try astype():
df.astype({'col1':'int', 'col2':'float'})
But it will fail if you have any invalid input. Better way:
df.apply(pd.to _numeric, errors='coerce')
Converts invalid input to NaN



# In-Depth Pandas
https://github.com/debaonline4u/Python_Programming/tree/master/pandas_and_data_visualization

# Convert Pandas to SQL
https://pandas.pydata.org/pandas-docs/stable/getting_started/comparison/comparison_with_sql.html

Especially handy: top n-rows per group:
-- Oracle's ROW_NUMBER() analytic function
SELECT * FROM (
  SELECT
    t.*,
    ROW_NUMBER() OVER(PARTITION BY day ORDER BY total_bill DESC) AS rn
  FROM tips t
)
WHERE rn < 3
ORDER BY day, rn;
