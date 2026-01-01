## 5. Change Request Communication

### Conversation 1: Receiving a Change Request

**Client:** We need to change the color scheme of the entire application. Can we do this before launch?

**You:** I understand you want to update the colors. Let me clarify a few things. What's driving this change?

**Client:** Our marketing team feels the current colors don't align with our brand guidelines.

**You:** I see. This is a significant change. We're currently 2 weeks from launch. Let me assess the impact.

**Client:** How long would it take?

**You:** Changing colors across the entire application involves updating CSS, testing on all pages, and ensuring accessibility standards are met. That's roughly 40 hours of work.

**Client:** So about one week?

**You:** Yes, plus we'd need another 2-3 days for thorough testing. This would push our launch date back by about 10 days.

**Client:** That's too much delay. Can we do it after launch?

**You:** Absolutely. We could launch as planned and deploy the color changes as an update the following week. That way, there's no delay to your launch.

**Client:** Let's do that. Please add it to the post-launch backlog.

**You:** Will do. I'll document this change request and send you the updated timeline.

---

### Conversation 2: Evaluating Change Request Feasibility

**Client:** Can we add a chat feature to the application? I know we're almost done, but customers are asking for it.

**You:** Chat functionality is definitely possible. Let me break down what this involves.

**Client:** Please do.

**You:** We'd need to implement real-time messaging, message storage, notification system, and user presence indicators. This is a major feature.

**Client:** How much time and cost are we talking about?

**You:** Development would take 6-8 weeks and cost approximately $30,000. Plus ongoing server costs of about $500 monthly.

**Client:** That's much more than I expected. Are there alternatives?

**You:** Yes, we could integrate a third-party chat service like Intercom or Zendesk. That would take 2 weeks and cost around $5,000 for integration, plus their monthly subscription.

**Client:** What's the monthly subscription cost?

**You:** Typically $100-300 depending on the number of users. Much more cost-effective than building from scratch.

**Client:** Let's go with the third-party integration. Can we do this before launch?

**You:** It would delay launch by 2 weeks. I recommend we launch first and add chat in version 1.1 next month.

**Client:** Agreed. Let's plan for that.

---

### Conversation 3: Handling Scope Change

**Client:** I know we agreed on 5 user roles, but we actually need 10 different roles with custom permissions.

**You:** Okay, let me understand the change. Originally we planned for Admin, Manager, User, Guest, and Support roles. What are the new roles you need?

**Client:** We need to add Department Head, Regional Manager, Auditor, Contractor, and Viewer roles.

**You:** Got it. This doubles the scope of the user management system. Each role needs different permissions configured and tested.

**Client:** Will this affect the timeline?

**You:** Yes, significantly. The original estimate was 2 weeks for 5 roles. For 10 roles, we're looking at 3.5 to 4 weeks.

**Client:** That's a 2-week delay. Can we speed it up?

**You:** We could deliver the original 5 roles on schedule, then add the additional 5 roles in a phase 2 release two weeks later.

**Client:** Would that work seamlessly for users?

**You:** Yes, it would be a smooth update. Existing users won't be affected, and new roles will simply become available.

**Client:** Perfect. Let's split it into two phases then.

**You:** Great. I'll update the scope document and send it for your approval.

---

### Conversation 4: Rejecting an Unreasonable Change Request

**Client:** We want to completely redesign the database structure. The current design isn't what we envisioned.

**You:** I understand your concern. However, we're already in the testing phase with live data in the system. Let me explain the implications.

**Client:** Go ahead.

**You:** Redesigning the database now would mean rebuilding most of the application from scratch. We'd lose approximately 4 months of work.

**Client:** That sounds extreme. Surely it's not that complicated?

**You:** The database is the foundation. Everything - APIs, business logic, frontend - is built on top of it. Changing it means changing everything connected to it.

**Client:** What would be the timeline and cost?

**You:** We're looking at 5-6 months and approximately $150,000. Plus we'd need to migrate all existing data, which carries risk.

**Client:** That's not feasible. What are our options?

**You:** I recommend we identify your specific pain points with the current structure. We can optimize and refactor specific areas without a complete redesign.

**Client:** Can you give me an example?

**You:** If query performance is the issue, we can add indexes and optimize queries. If it's data relationships, we can adjust specific tables without touching everything.

**Client:** Let me schedule a meeting to discuss the actual problems we're facing.

**You:** Perfect. Once I understand the root issues, I can propose targeted solutions that won't derail the project.

---

### Conversation 5: Documenting Change Request

**Client:** We'd like to add two-factor authentication to the login process.

**You:** That's a great security enhancement. Let me document this properly. Is this for all users or specific roles?

**Client:** All users should have the option, but it should be mandatory for admin users.

**You:** Understood. What methods should we support - SMS, email, or authenticator apps?

**Client:** Authenticator apps like Google Authenticator.

**You:** Perfect. And what's your preferred timeline for this?

**Client:** As soon as possible, but I understand it's not in the current scope.

**You:** Right. Let me prepare a change request document with effort estimates, timeline, and cost. I'll have that to you by tomorrow.

**Client:** Great. Also include the security benefits in the document.

**You:** Absolutely. I'll outline how this enhances security and protects user accounts.

**Client:** Thank you. I'll need to present this to the board for approval.

**You:** No problem. I'll make sure the document is comprehensive and includes all the information you need.

---

### Conversation 6: Prioritizing Multiple Change Requests

**You:** Hi Rebecca, we've received 5 change requests this week. We need to prioritize them.

**Client:** Yes, let's go through them.

**You:** First is the two-factor authentication we discussed. Second is adding PDF export. Third is integrating with Salesforce. Fourth is adding dark mode. Fifth is improving search functionality.

**Client:** They're all important. Can we do them all?

**You:** Not simultaneously. Each requires time and resources. We need to rank them by business value and urgency.

**Client:** Two-factor authentication is critical for security. That's number one.

**You:** Agreed. What about the others?

**Client:** Salesforce integration would unlock a major client deal. That's number two.

**You:** That makes sense. For the remaining three, I suggest we batch them for the next release cycle. Does that work?

**Client:** Yes. Can you give me estimates for the top two?

**You:** Two-factor authentication: 3 weeks, $12,000. Salesforce integration: 4 weeks, $18,000.

**Client:** When can we start?

**You:** We can start two-factor authentication next Monday and Salesforce integration immediately after.

**Client:** Approved. Please send the formal change orders.

---

### Conversation 7: Change Request Impact on Budget

**Client:** We want to add video upload functionality. What's the cost?

**You:** Video uploads require additional infrastructure. Let me break down the costs.

**Client:** Please do.

**You:** Development cost is $8,000. However, video storage and processing will add significant ongoing costs.

**Client:** How much monthly?

**You:** It depends on usage. For 1000 videos per month at average 5 minutes each, you're looking at approximately $500-800 monthly.

**Client:** That's more than expected. Is there a cheaper option?

**You:** We could implement file size limits and video compression to reduce costs. That could bring it down to $300-400 monthly.

**Client:** What's the trade-off?

**You:** Users can only upload videos up to 10 minutes and 100MB. Quality would be compressed but still acceptable.

**Client:** That's reasonable. What about the development cost - can we reduce that?

**You:** We could use a third-party service like Vimeo API instead of building it ourselves. Development would be $4,000, and their service costs $75 monthly.

**Client:** That sounds better. What are the limitations?

**You:** We'd be dependent on their service, and branding might show "Powered by Vimeo" unless we upgrade their plan.

**Client:** I can live with that. Let's go with the Vimeo integration.

**You:** Great. I'll prepare the updated budget and timeline.

---

### Conversation 8: Emergency Change Request

**Client:** We just found out our competitor launched a feature we don't have. We need to add it ASAP!

**You:** I understand the urgency. What feature are we talking about?

**Client:** Social media sharing. Users can share their achievements on Facebook and Twitter.

**You:** Okay, let me assess this quickly. This would require API integration with both platforms and UI changes.

**Client:** How fast can we do this?

**You:** If we drop everything else and focus on this, we could have it ready in 1 week.

**Client:** What would we be delaying?

**You:** The reporting dashboard scheduled for next week and the bug fixes planned for this sprint.

**Client:** Those bugs - are they critical?

**You:** Two are minor UI issues. One is a performance bug that affects load times by 2 seconds.

**Client:** Can we fix the performance bug and do the social sharing?

**You:** Yes, if we postpone the dashboard to the following week. We'd need to work some overtime.

**Client:** Approved. Please proceed with social sharing and the performance fix.

**You:** Will do. I'll send you daily updates on progress.

---