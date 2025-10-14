Bookmarks Web Application
Privacy-first bookmark manager with WhatsApp chat integration. Built with React + Supabase + n8n.
Prerequisites
•	Supabase account
•	Lovable account 
•	n8n instance
•	Whapi.cloud account (for WhatsApp)
Environment Variables
Web Application
Create .env or configure in Lovable:
VITE_SUPABASE_URL=https://xxxxx.supabase.co
VITE_SUPABASE_ANON_KEY=
n8n Workflow
Configure in n8n credentials:
•	Supabase URL: https://xxxxx.supabase.co
•	Supabase Service Key: (service_role key)
•	Whapi Token: Your Whapi.cloud API token
Setup & Run
1. Database Setup (Supabase)
-- Run in Supabase SQL Editor  - https://github.com/aditeebiyani/AI_Projects/blob/main/Bookmarked/Supabase_SQL_Queries

2. Run Web App
Lovable (Recommended):
1.	Connect repo to Lovable
2.	Add environment variables in dashboard
3.	Deploy automatically
3. Setup n8n Workflow
1.	Import workflows into n8n - 
https://github.com/aditeebiyani/AI_Projects/blob/main/Bookmarked/Bookmarked_withToolUse.json
https://github.com/aditeebiyani/AI_Projects/blob/main/Bookmarked/Bookmarked_withoutToolUse.json

2.	Configure credentials (Supabase + Whapi)
3.	Activate workflow
4.	Copy webhook URL → Configure in Whapi.cloud
Test Steps
1. Web Application Tests
Authentication:
✓ Sign up with email/password
✓ Verify email received
✓ Log in with credentials
✓ Log out and verify redirect
Bookmarks CRUD:
✓ Add bookmark with title, URL, tags
✓ Verify bookmark appears in table
✓ Edit bookmark (change title)
✓ Toggle reading flag
✓ Delete bookmark with confirmation
Filters & Search:
✓ Search by title keyword
✓ Filter by single tag
✓ Filter by multiple tags
✓ Toggle "Show Reading List Only"
✓ Clear all filters
Profile:
✓ View profile page
✓ Add phone number (+1234567890)
✓ Save and verify success toast
2. Database Security Tests
Run in Supabase SQL Editor with different user sessions:
-- Test 1: Cross-user read (should return 0 rows)
-- Log in as User A, then execute:
SELECT * FROM bookmarks WHERE user_id = '<user_b_id>';

-- Test 2: Unauthorized update (should update 0 rows)
UPDATE bookmarks SET title = 'Hacked' WHERE user_id != auth.uid();

-- Test 3: Unauthenticated access
-- Without JWT token, try API call:
curl https://xxxxx.supabase.co/rest/v1/bookmarks \
  -H "apikey: <anon_key>"
-- Expected: Empty array or 401
3. WhatsApp Integration Tests
Setup:
1.	Link phone number in web app profile
2.	Send WhatsApp message to configured number
Test Commands:
✓ "save https://example.com #ai #tools"
   → Verify bookmark created in web app

✓ "show my reading list"
   → Verify list returned with correct items

✓ "search: productivity"
   → Verify relevant bookmarks returned

✓ "add to reading list: https://blog.com"
   → Verify reading flag set to true
Common Issues
Problem: RLS blocking all queries
Solution: Ensure auth.uid() matches user_id in JWT token
Problem: n8n workflow not receiving messages
Solution: Verify webhook URL configured in Whapi.cloud
Problem: Phone number lookup fails
Solution: Use E.164 format (+countrycode + number), ensure unique in database
Problem: Tags not filtering correctly
Solution: Tags are case-sensitive, use exact match or lowercase normalization
Support: Open GitHub issue or check Supabase logs for debugging

