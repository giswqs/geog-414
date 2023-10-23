---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.15.2
---

# Introduction

## Spatial databases

A database management system (DBMS) allows users to store, insert, delete, and update information in a database. Spatial databases go a step further because they record data with geographic coordinates.

From Esri Geodatabase to PostGIS, spatial databases have quickly become the primary method of managing spatial data.

To learn more about spatial databases, check out the resources below:

- [Wikipedia: Spatial database](https://en.wikipedia.org/wiki/Spatial_database)
- [7 Spatial Databases for Your Enterprise](https://engage.safe.com/blog/2021/11/7-spatial-databases-enterprise)
- [GISGeography: Spatial Databases – Build Your Spatial Data Empire](https://gisgeography.com/spatial-databases/)
- [Esri: What is a geodatabase?](https://pro.arcgis.com/en/pro-app/latest/help/data/geodatabases/overview/what-is-a-geodatabase-.htm)
- [Introduction to PostGIS](https://postgis.net/workshops/postgis-intro)
- [PostGEESE? Introducing The DuckDB Spatial Extension](https://duckdb.org/2023/04/28/spatial.html)

## DuckDB

[DuckDB](https://duckdb.org) is an in-process SQL OLAP database management system. It is designed to be used as an embedded database in applications, but it can also be used as a standalone SQL database.

* _In-process SQL_ means that DuckDB’s features run in your application, not an external process to which your application connects. In other words: there is no client sending instructions nor a server to read and process them. SQLite works the same way, while PostgreSQL, MySQL…, do not.
* _OLAP_ stands for OnLine Analytical Processing, and Microsoft defines it as a technology that organizes large business databases and supports complex analysis. It can be used to perform complex analytical queries without negatively affecting transactional systems. 

DuckDB is a great option if you’re looking for a serverless data analytics database management system. 

![](https://i.imgur.com/BEDGstx.png)

![](https://i.imgur.com/mFpqp5I.png)

To me, DuckDB feels like the [fresh air](https://tenor.com/view/breeze-nicolas-cage-con-air-smile-gif-14443307) in the database space. It is a modern database that is built from the ground up to be fast, lightweight, and easy to use. It is also open source and free to use. It has a [spatial extension](https://duckdb.org/docs/extensions/spatial.html) that allows you to perform spatial queries and analysis.

## Installation

### Command line

Go to <https://duckdb.org/#quickinstall> and click the link under the **Command Line** section that corresponds to your operating system. Save the file to your computer and unzip it. You should now have a `duckdb` executable file. It is recommended that you move this file to a directory that is in your `PATH` environment variable.

For Windows users, it is recommended that you move the `duckdb` executable file to the `C:\Windows` directory. If you do not have permission to move the file to this directory, you can create a new directory under your user directory and add it to your `PATH` environment variable by following the instructions below:

1. Open the Start Menu and search for "environment variables"
2. Click on "Edit the system environment variables"
3. Click on "Environment Variables..."
4. Under "System variables", select the "Path" variable and click "Edit..."
5. Click "New" and enter the path to the directory where you saved the `duckdb` executable file
6. Click "OK" to save your changes

### Python

To install the required Python package for this course, it is recommended that you create a new conda environment and install the package using the following command:

```bash
conda create -n geo python=3.11
conda activate geo
conda install -c conda-forge mamba
mamba install -c conda-forge python-duckdb duckdb-engine jupysql leafmap
```

You can also install the packages using `pip`:

```{code-cell}
%pip install duckdb duckdb-engine jupysql leafmap
```

To test if the installation was successful, open a new terminal and run the following command:

```bash
conda activate geo
python -c "import duckdb; print(duckdb.__version__)"
```

If the installation was successful, you should see the version number of the DuckDB package printed to the terminal. Otherwise, you will see an error message. If you see an error message, please check the installation instructions above and try again.

## Setting up VS Code

To make it easier to run DuckDB commands and SQL queries, it is recommended that you create a keyboard shortcut to run selected text in VS Code. To do this, open VS Code and press `Ctrl+Shift+P` to open the Command Palette. Then, search for "Preferences: Open Keyboard Shortcuts (JSON)" and press `Enter`. This will open the `keybindings.json` file. Add the following keyboard binding within the square brackets:

```json
    {
        "key": "shift+ctrl+enter",
        "command": "workbench.action.terminal.runSelectedText",
    }
```

Click the Terminal tab at the bottom of the VS Code window and run the following command:

```bash
duckdb
```

To test if the keyboard shortcut is working, create a new file with the `.sql` extension within VS Code. Then, copy and paste the following SQL query into the file:

```sql
SELECT * FROM duckdb_extensions();
```

Select the text and press `Shift+Ctrl+Enter`. If the keyboard shortcut is working, you should see the following output in the terminal:

![](https://i.imgur.com/5mGW8hj.png)

To stop the DuckDB process, press `Ctrl+C` in the terminal. To exit the terminal, press `Ctrl+D`.

### DBeaver SQL IDE

[DBeaver](https://dbeaver.io) is a powerful and popular desktop sql editor and integrated development environment (IDE). It has both an open source and enterprise version. It is useful for visually inspecting the available tables in DuckDB and for quickly building complex queries. To install DBeaver, go to <https://dbeaver.io/download> and download the Community Edition for your operating system. Once installed, follow the instructions [here](https://duckdb.org/docs/guides/sql_editors/dbeaver.html) to connect to DuckDB. You can then run SQL queries in DBeaver and view the results in a table format.

![](https://i.imgur.com/59clo36.png)

## References

- [Forget about SQLite, Use DuckDB Instead — And Thank Me Later](https://towardsdatascience.com/forget-about-sqlite-use-duckdb-instead-and-thank-me-later-df76ee9bb777)
- [DuckDB: The indispensable geospatial tool you didn’t know you were missing](https://medium.com/radiant-earth-insights/duckdb-the-indispensable-geospatial-tool-you-didnt-know-you-were-missing-5fe11c5633e5)
- [DuckDB's Spatial Extension](https://tech.marksblogg.com/duckdb-gis-spatial-extension.html)
- [DuckDB Tutorial for Beginners](https://motherduck.com/blog/duckdb-tutorial-for-beginners/)
