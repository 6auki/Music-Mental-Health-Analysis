# Music Mental Health Analysis

A comprehensive data analysis project exploring the correlations between music preferences and self-reported mental health using Neo4j graph database and interactive dashboards.

## Project Overview

This project analyzes the **"Music & Mental Health Survey Results"** dataset from Kaggle to identify correlations between individual music taste and mental health conditions. The analysis is built using Neo4j graph database with interactive dashboards for data visualization.

## Key Findings

- **736 survey respondents** across different age groups and music preferences
- Analysis of **4 major mental health conditions**: Anxiety, Depression, Insomnia, and OCD
- Correlation patterns between music genres, listening habits, and mental health severity
- Interactive dashboards revealing insights across age groups, streaming preferences, and musical activities

## Database Schema

### Nodes (5 types)
- **Person**: Survey respondents with demographics and music habits
- **StreamingService**: Music platforms (Spotify, Pandora, YouTube Music)
- **Genre**: Music genres (Rock, Pop, Jazz, Lofi, Metal, etc.)
- **MentalHealthIssue**: Mental health conditions with severity metrics
- **WorkingHabit**: Listening behaviors and exploration patterns

### Relationships (4 types)
- **USES**: Person → StreamingService (with `hours_per_day`)
- **LISTENS_TO**: Person → Genre (with frequency scores 1-5)
- **HAS_CONDITION**: Person → MentalHealthIssue (with severity levels)
- **HAS_HABIT**: Person → WorkingHabit (exploration and work listening patterns)

## Dashboard Pages

### 1. General Data Analysis (Overview)
- Age distribution of respondents
- Primary streaming service usage
- Daily listening hours breakdown
- Music listening habits (while working, instruments, composition)
- Genre preferences and exploration patterns

### 2. Deeper Analysis (Detailed Tables)
- Age group vs. genre frequency correlations
- Streaming service preferences by age
- Instrumentalist and composer genre preferences
- Foreign language music listening patterns

### 3. Mental Health Analysis
- Music effects on mental health (Improve/Worsen/No effect)
- Age-specific mental health severity patterns
- Impact of daily listening hours on mental health
- Composer vs. non-composer mental health comparison
- Work listening habits and mental health correlation

### 4. Genre-Specific Analysis
- Mental health patterns for Rock, Metal, Pop, and Lofi listeners
- Parameter-driven genre selection for custom analysis

### 5. Cross-Genre Preferences
- Secondary genre preferences for primary genre listeners
- Top 3 alternative genres by primary preference
- Interactive genre exploration

### 6. Graph Visualizations
- Person nodes and relationships
- Streaming service usage patterns
- Mental health condition networks

## Repository Structure

```
music-mental-health-analysis/
├── README.md
├── data/
│   ├── music.csv                 # Raw survey data (736 rows, 32 columns)
│   └── export.csv                # Neo4j export data
├── dashboard/
│   └── dashboard.json            # Neo4j dashboard configuration
├── docs/
│   └── project-documentation.pdf # Detailed project documentation
```

## Technologies Used

- **Database**: Neo4j Graph Database
- **Visualization**: Neo4j Bloom & NeoDash
- **Data Processing**: Cypher Query Language
- **Charts**: Bar charts, Pie charts, Line charts, Tables, Graph visualizations

## Dataset Information

**Source**: Kaggle - "Music & Mental Health Survey Results"

**Size**: 736 respondents, 32 attributes

**Key Attributes**:
- Demographics: Age
- Music Habits: Hours per day, streaming service, favorite genre
- Musical Activities: Instrumentalist, composer, exploratory listening
- Mental Health: Anxiety, Depression, Insomnia, OCD severity scores
- Preferences: Genre frequency ratings, foreign language music, BPM preferences

## Getting Started

### Prerequisites
- Neo4j Desktop or Neo4j Browser
- NeoDash plugin for interactive dashboards

### Setup Instructions

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/music-mental-health-analysis.git
   cd music-mental-health-analysis
   ```

2. **Import data into Neo4j**
   - Create a new Neo4j database
   - Import the `music.csv` file using Neo4j's data import tools
   - Run the data transformation queries to create the graph schema

3. **Load the dashboard**
   - Install NeoDash plugin in Neo4j Desktop
   - Import the `dashboard.json` file into NeoDash
   - Connect to your Neo4j database

4. **Explore the analysis**
   - Navigate through the 6 dashboard pages
   - Use parameter selectors for interactive filtering
   - Analyze correlations between music preferences and mental health

## Sample Cypher Queries

### Find average mental health severity by genre
```cypher
MATCH (p:Person {favorite_genre: 'Rock'})-[r:HAS_CONDITION]->(m:MentalHealthIssue)
RETURN 
    m.type AS mental_health_issue,
    AVG(r.severity) AS avg_severity
ORDER BY mental_health_issue
```

### Analyze streaming service usage by age group
```cypher
MATCH (p:Person)-[r:USES]->(s:StreamingService)
WITH 
   CASE 
       WHEN p.age < 20 THEN 'Under 20'
       WHEN p.age < 30 THEN '20-29'
       WHEN p.age < 40 THEN '30-39'
       ELSE '40+'
   END as age_group,
   s.name as service,
   COUNT(*) as count
RETURN age_group, service, count
ORDER BY age_group, count DESC
```

### Explore genre cross-preferences
```cypher
MATCH (p:Person)-[l:LISTENS_TO]->(g:Genre)
WHERE p.favorite_genre = 'Pop' AND g.name <> 'Pop' AND l.frequency <> 'Never'
WITH g.name AS genre, AVG(l.frequency_score) AS avgFrequency
ORDER BY avgFrequency DESC
LIMIT 3
RETURN genre, avgFrequency
```

## Key Insights Discovered

- **Age Patterns**: Different age groups show varying mental health severity levels
- **Genre Correlations**: Certain music genres correlate with specific mental health patterns
- **Listening Duration**: Hours of daily music consumption impact mental health differently
- **Musical Activity**: Instrumentalists and composers show distinct mental health profiles
- **Work Habits**: Listening to music while working affects mental health outcomes

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- Dataset source: Kaggle "Music & Mental Health Survey Results"
- Neo4j community for graph database tools
- NeoDash for interactive dashboard capabilities

---

This project demonstrates the power of graph databases in analyzing complex relationships between music preferences and mental health, providing valuable insights for music therapy and mental health research.
