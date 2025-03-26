# Detecting the First Source of News with Claude and MCP Servers

This protocol uses Claude desktop along with four MCP servers to determine the earliest published source of a news story. The MCP servers and their roles are:

- [Brave-Search](https://github.com/modelcontextprotocol/servers/tree/main/src/brave-search): Retrieves news search results.
- [Sequential-Thinking](https://github.com/modelcontextprotocol/servers/tree/main/src/sequentialthinking): Orders sources by publication time.
- [Puppeteer](https://github.com/modelcontextprotocol/servers/tree/main/src/puppeteer): Scrapes and extracts metadata from web pages.
- [Tavily](https://github.com/tavily-ai/tavily-mcp): Visualizes and verifies the publication timeline.

## Protocol Steps

### 1. Input & Query Definition
- **User Input:** Prompt the user to specify the exact news headline or key details (e.g., keywords, event date/time).
- **Query Construction:** Build a search query that includes the news headline and relevant keywords. If available, incorporate time constraints (e.g., "news [headline] published before [date/time]") to filter for early reports.

### 2. Data Collection with Brave-Search
- **Action:** Use the Brave-Search MCP to search for the specified news.
- **Instruction:** Direct Brave-Search to retrieve a broad set of results related to the news query.
- **Expected Output:** A list of candidate URLs and article summaries, ideally with publication dates or snippets indicating timing.

### 3. Scraping and Metadata Extraction with Puppeteer
- **Action:** Pass the candidate URLs obtained from Brave-Search to Puppeteer.
- **Instruction:** Instruct Puppeteer to:
  - Navigate each URL.
  - Extract key metadata: publication date/time, author (if available), article content, and any timestamp details.
  - Save the extracted data in a structured format (e.g., table or JSON object).
- **Error Handling:** Flag pages lacking clear publication metadata for manual review.

### 4. Chronological Ordering using Sequential-Thinking
- **Action:** Input the extracted metadata into the Sequential-Thinking module.
- **Instruction:** Use Sequential-Thinking to:
  - Parse and validate each publication timestamp.
  - Order candidate news sources from earliest to latest.
  - Identify and handle any inconsistencies (e.g., reprints or aggregated content) by comparing publication details.
- **Expected Output:** A sorted list of news sources with clearly identified publication times.

### 5. Timeline Visualization and Verification with Tavily
- **Action:** Utilize the Tavily module to analyze the publication timeline.
- **Instruction:** Direct Tavily to:
  - Create a timeline visualization based on the ordered data.
  - Highlight the earliest timestamp and corresponding news source.
  - Provide cross-verification by checking additional contextual details or citations that confirm the publication time.
- **Expected Output:** A visual and textual confirmation of the earliest source, complete with detailed metadata.

### 6. Final Confirmation and Reporting
- **Action:** Review outputs from Sequential-Thinking and Tavily to determine the "first source" of the news.
- **Instruction:** Confirm that the earliest published source is:
  - Not merely an aggregated or syndicated report.
  - The original or primary publication.
- **Reporting:** Output the following details:
  - URL of the first source.
  - Publication timestamp.
  - Additional verified details (e.g., author, location, content snippet).
- **Fallback:** If publication dates are ambiguous or missing, flag the record and suggest a manual review.

### 7. Error Handling & Iteration
- **Action:** Throughout the process, ensure that each MCP monitors for errors.
- **Instruction:** If any step (search, scraping, ordering, visualization) fails or returns ambiguous data:
  - Log the error.
  - Provide instructions for a re-run with adjusted parameters (e.g., refining the search query or re-scraping).
- **Iteration:** Allow the process to repeat iteratively if new information or corrections are required.

## Integration Summary
- **Brave-Search:** Retrieve a comprehensive set of news results using a carefully constructed query.
- **Puppeteer:** Automate browser tasks to extract detailed metadata from each candidate URL.
- **Sequential-Thinking:** Analyze and order the candidate sources by publication time logically.
- **Tavily:** Visualize the timeline to confirm and highlight the earliest published source.
