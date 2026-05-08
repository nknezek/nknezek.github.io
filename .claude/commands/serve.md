Start the Jekyll development server on port 4000, killing any existing process on that port first.

Run these steps:
1. Kill any process currently using port 4000: `lsof -ti :4000 | xargs kill -9 2>/dev/null; true`
2. Start the server: `bin/jekyll serve --host 0.0.0.0 --port 4000`
3. Confirm the server started by checking for "Server address" in the output
4. Tell the user the site is available at http://localhost:4000
