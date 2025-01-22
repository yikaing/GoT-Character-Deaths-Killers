# GoT-Character-Deaths-Killers
# Overview
In our project analyzing Game of Thrones character deaths, we aim to gain insights into the power dynamics within the series by studying the relationships between characters, the houses they belong to, and the individuals responsible for the killings. Some goals that we would like to achieve with our project are identifying patterns in character deaths throughout the series and identifying patterns in internal and external conflicts between the houses of Westeros.

# Goal
The project focuses on discovering interesting insights about the deaths in Game of Thrones through data analysis. We address the following questions:

- Task1: How do the death of prominent characters influence the ratings?

- Task2: Which regions are targeted most often?

We will visualize our findings to present the information in a clear and engaging way for users.

# Workflow
![image](https://github.com/user-attachments/assets/c8c7ec4c-8345-4ffe-8ff2-3fd1aff270e0)

We started by downloading raw data from various sources and importing it into RStudio/Jupyter. Since the data came in multiple formats—CSV, JSON, and XLSX—we used proper functions to load the data into the project database. Next, we cleaned and formatted the data, and further validated the data. Then, we merged them into a single, unified data frame. We then queried this data frame to extract relevant information for visualizing and identifying trends.

# Database Design
![image](https://github.com/user-attachments/assets/d014d6ae-5042-4515-af6b-85d70854136d)

**Episode - Death:** The episode and death data sets have a shared common columns: season and episode. This allows us to map each episode to the on-screen deaths that occurred within it.

**Death - Character:** Ideally, there should be a one-to-one mapping between the death data set and the character data set. However, we lacked sufficient data to know clear relationships in all cases. Some characters in the show are unnamed, and the data set uses generic terms like “Stark Soldier” to refer to them, making the mapping relationship ambiguous. As a result, we excluded entries where the mapping between the two data sets was uncertain.

**Death - Location/Region - Map Location:** We planned to link death locations from the death data set to corresponding entries in the map location data set. However, the names did not have a direct one-to-one relationship. For example, “Castle Black” (death location) is part of “The Wall” (map location), but we lacked sufficient data to make these connections. To address this, we used AI to generate a look-up table. Since the AI-generated data might not be fully reliable, we manually verified a sample of the entries. Based on these checks, we implemented the look-up table to help us join the data sets.

# Data Sources
We used five data sources, one of which was generated by AI. The remaining data were compiled by three individual fans: David Murphy, Mohammad Reza Ghari, and Jeffery Lancaster.

## Death Data
- Description: This data set documents every on-screen death in Game of Thrones. It includes information on who killed whom, the method applied, the location, and the episode of death. The author sets specific criteria to determine what qualifies as an on-screen death, and more details are provided [here](https://datasaurus-rex.com/inspiration/dsrgallery/gotviz-mkiii).
- Source: The data is collected by **David Murphy**.
- Link: https://data.world/datasaurusrex/game-of-thones-deaths
- Challenges:
  - **Killers without houses:** We identified that some killers do not belong to any houses. Initially, we considered replacing this with ‘N/A.’ However, we recognized that it is reasonable for some killers not to be associated with any houses. We decided to leave it “None”.
  - **Repeated names:** Some death names are repeated. This is because certain characters in the show do not have specific names and are referred to by general terms, such as “Tribesman” or “Wildling.” Despite this, each entry still represents a single death. We decided to retain these entries in the table.
 
![image](https://github.com/user-attachments/assets/7891b80c-fd22-412e-9e0f-b929554ee961)

## Episode Data
- Description: This data set provides detailed episode-level data for Game of Thrones. It includes key information such as season and episode numbers, titles, release dates, IMDb ratings, vote counts, reviews, and key contributors like writers and actors.
- Source: The data is collected by **Mohammad Reza Ghari**. He scraped the data from IMDB & Wikipedia.
- Link: https://www.kaggle.com/datasets/rezaghari/game-of-thrones
- Challenges:
  - **Non-standard date format:** The data uses the “12-Apr-2004” format, which is a non-standard date format in R’s tidyverse. However, R can still recognize this format, but it requires using the as.Date() function to convert it into a proper date object.

![image](https://github.com/user-attachments/assets/15f4243e-e8a4-4f9e-90b8-456afff3e7e0)

## Location Data
- Description: This data set contains geographical coordinates for various regions on the Ice and Fire world map.
- Source: The data set is compiled by Jeffery Lancaster, who sourced information from [the Game of Thrones Wiki](https://gameofthrones.fandom.com/wiki/Wiki_of_Westeros) and manually encoded each location.
- URL: https://github.com/jeffreylancaster/game-of-thrones/blob/master/data/opening-locations.json
- Challenges:
  - **Non-rectangular data:** The location data comes from a JSON file, which is not a rectangular data. It consists of two key-value pairs: “note” and “locations.” Fortunately, the “locations” data itself is in a rectangular format, making it easy to extract and use.

![image](https://github.com/user-attachments/assets/94de8c00-cb39-4f79-964f-b5d486778486)

## Characters Data
- Description: This data set provides detailed insights into the Game of Thrones characters, including the total number of episodes they appeared in, along with their first and final appearance years on the show.
- Source: The data is collected by **Mohammad Reza Ghari**. He scraped the data from IMDB & Wikipedia.
- URL: https://www.kaggle.com/datasets/rezaghari/game-of-thrones?select=characters_v4.csv
- Challenges:
  - **Repeated characters:** The character data includes repeated entries for unnamed characters, such as “Stark Soldier.” Since these generic terms represent distinct individuals in the show, we decided to retain all entries in the data set.

![image](https://github.com/user-attachments/assets/4deb28cb-8d93-4f85-9d44-68be843e0494)

## Look-up table for mapping locations and regions
- Description: The mapping relationship between death locations and map regions.
- Source: Due to the unavailability of look-up table between death locations and map regions, we generated this data using the AI tool, Perplexity. We selected Perplexity because it can use web searches to generate answers and provides references for its responses, ensuring greater accuracy and up-to-date information.
- *Note: Since the prompting process relies on the location and death data, it will be explained in detail in the data importing section.
- Challenges:
  - **Refine AI Answer:** While AI is a powerful tool for accessing data we may lack, it can sometimes produce inaccurate results. To improve accuracy, we initially told Perplexity, “If a location doesn’t match any listed regions, leave it blank.” However, the output only matched locations to regions when their names were nearly identical, leaving many entries blank. This approach did not produce good results. We then revised our prompt, asking the AI to infer the best matches based on its knowledge. To verify these results, we manually reviewed a sample of matches by searching for location names online and referring to sources such as the Wiki of Westeros fandom site. The results seemed to fit well. Although this method isn’t entirely reliable, the answer seemed good enough to meet our needs.
    
      - **First Prompt:**

        Categorize each of the following Game of Thrones locations into their respective regions and present the output in a table. If a location does not belong to any of the listed regions, **leave it blank.**

        **Result:**
        
        ![image](https://github.com/user-attachments/assets/88db0018-7d43-4594-9736-2b65771c7bbe)
        
      - **Second Prompt:**

        Categorize each of the following Game of Thrones locations into their respective regions and present the output in a table. If a location does not have a direct one-to-one mapping, **use your best knowledge to infer its region.**

        **Result:**

        ![image](https://github.com/user-attachments/assets/85f30841-ac80-41e1-9e9e-9bbc3b652677)

# Import Data
All datasets were imported into RStudio (using R and SQL) and Jupyter Hub (using Python) preprocessing and cleaning. Data types were standardized to maintain consistency across tools and ensure smooth integration during analysis. Validation queries and checks were performed in each environment to confirm the accuracy of the imported data. The detailed code for data imports in all three languages can be found in the data_import folder.

# Combine Data into One Data Frame
The imported datasets were merged into a single data frame to create a cohesive structure for analysis and visualization. Relationships between datasets were established using key attributes, ensuring the combined data was ready for further exploration. The code for this process is provided in the data_combination folder.

# Data Analysis and Visualization
The analysis focused on two key tasks: examining how the deaths of prominent characters influenced episode ratings and identifying which regions were most frequently targeted. These insights were visualized to reveal patterns and trends in the data. The code and visualizations for these tasks are available in the data_analysis_visualization folder.

# Project Challenges
- **Handling Many-to-Many Mappings**

  While processing the data, we encountered issues when trying to merge the death and character datasets, leading to duplicate names and causing a many-to-many mapping warning. This issue occurred because the original character names included repeated entries, specifically due to the presence of non-individual character names, such as Wildling, which represented a collective group rather than a specific character. We decided to remove the duplicated names from the dataset, which resolved the many-to-many conflict and allowed for a cleaner join between the datasets.

- **Correcting Coordinate Alignment on the Map**

  During the creation of a scatterplot showing death count points on the map, we noticed that some points were located in the sea rather than on land. Identifying the issue, we analyzed the reason by examining the map location data to understand how the coordinates were defined. We discovered that the coordinates followed a standard 2D screen coordinate system, in which the origin is at the top-left corner. Additionally, the map’s scale was specified in the notes, indicating that the DPI was 96. With this information, we transformed our coordinates to align with the data’s coordinate system. This adjustment allowed us to correctly plot the points on the map.

- **Bridging the Gap Between Datasets**

  We encountered a challenge in joining the death data and map location data due to a lack of sufficient information. After searching online for a while, we realized that acquiring this data might not be possible. We then considered leveraging the power of AI to bridge the gap. While using AI introduced some uncertainty in the information, we manually examined the results. After several checks, we determined that the AI-generated data could serve as a useful source to help connect the data sets. Given that there were 41 rows involved, this approach reduced the effort needed to assign and search for data manually.

- **Loading JSON data into DuckDB**

  Another challenge we faced was loading JSON data into DuckDB while working on the SQL part. Initially, we encountered errors when trying to use dbWriteTable() with data loaded using fromJSON() and we found out that this was due to the presence of nested structures. The fromJSON() function often returns a list with hierarchical data, while dbWriteTable() requires a flat data frame format, which DuckDB cannot handle directly if the input contains nested lists. We solved this issue by flattening the JSON data into a data frame format, ensuring all nested elements were converted into a tabular structure. Once flattened, the data was compatible with dbWriteTable(), allowing us to successfully write it to DuckDB.

  Instead of relying on external R functions like fromJSON() and dbWriteTable(), we utilized DuckDB’s native read_json_auto function to directly load the JSON file into a DuckDB table. This method handled the hierarchical JSON structures automatically, eliminating the need for manual flattening. Additionally, we used the UNNEST function to process the nested locations field, creating individual rows for each object in the array. By extracting specific fields with the ->> operator, we transformed the data into a usable table named locations.

- **Data Consistency and Matching During Joins**

  While also working on the SQL part, the region column was empty after joining, indicating that the location values were not matching between tables. We realized that SQL joins are typically case-sensitive, causing issues when values do not match due to capitalization differences, and extra spaces in the location column between tables led to join failures. We solved this problem by applying transformations to standardize the values before performing the join. Specifically, we used functions like LOWER() to convert all text to lowercase and TRIM() to remove any extra spaces. This ensured that values from both tables matched regardless of case or formatting inconsistencies, allowing for a successful join and populating the region column correctly.
