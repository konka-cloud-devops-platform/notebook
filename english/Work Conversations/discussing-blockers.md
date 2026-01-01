## 4. Discussing Blockers

### Conversation 1: Identifying a Blocker Early

**You:** Hey Maria, I need to flag a blocker before it becomes a bigger issue.

**Manager:** What's going on?

**You:** I'm working on the database migration, but I don't have access to the production database credentials. Without them, I can't complete the migration script testing.

**Manager:** Who has those credentials?

**You:** The senior DBA, but he's on vacation this week. I didn't realize this until I started the task this morning.

**Manager:** Can someone else provide access?

**You:** Yes, either you or the IT security team can grant temporary access. I just need read-only access to test the queries.

**Manager:** I'll reach out to IT security right now. How soon do you need this?

**You:** Ideally within the next 2 hours to stay on schedule. If it takes longer, I can work on other tasks and come back to this.

**Manager:** Let me see what I can do. I'll get back to you within 30 minutes.

**You:** Perfect, thank you.

---

### Conversation 2: Blocker Due to External Dependency

**You:** Quick update on the API integration task - I've hit a blocker.

**Team Lead:** What's blocking you?

**You:** The third-party payment gateway API is returning 500 errors. I've tried multiple times and checked our implementation - everything on our side is correct.

**Team Lead:** Have you contacted their support?

**You:** Yes, I opened a ticket an hour ago. Their support team says they're investigating a platform issue on their end.

**Team Lead:** What's their ETA?

**You:** They couldn't give an exact time, but said "within 24 hours." This blocks us from completing the integration testing.

**Team Lead:** Can you work around it for now?

**You:** I can use their sandbox environment to continue development, but we can't do final testing or deploy to production until their API is working.

**Team Lead:** Okay, continue with sandbox for now. Keep following up with their support every 3-4 hours for updates.

**You:** Will do. I'll also document everything so we can resume quickly once it's resolved.

**Team Lead:** Good thinking.

---

### Conversation 3: Technical Blocker

**You:** I need some help with a technical blocker I'm facing.

**Senior Engineer:** Sure, what's the issue?

**You:** I'm trying to deploy the application to Kubernetes, but the pods keep crashing with an "Out of Memory" error.

**Senior Engineer:** Have you checked the memory limits?

**You:** Yes, the container has 2GB memory limit configured. The application's normal usage is around 1.5GB, so it should be sufficient.

**Senior Engineer:** Is there a memory leak?

**You:** That's what I'm trying to determine. I've been monitoring for 2 hours and memory usage keeps climbing until it hits the limit and crashes.

**Senior Engineer:** Did you check if there are memory-intensive operations during startup?

**You:** Good point. Let me check the startup logs... Actually, yes! There's a data initialization process that loads a large dataset into memory.

**Senior Engineer:** That's probably it. Can you either increase the memory limit temporarily or lazy-load that data?

**You:** I'll increase it to 4GB for now and discuss with the dev team about lazy-loading for a permanent fix.

**Senior Engineer:** Perfect. Let me know if that resolves it.

**You:** Will do, thanks for the guidance!

---

### Conversation 4: Resource Blocker

**[Slack Message to Manager]**

**You:** @Manager I need to escalate a resource blocker for the infrastructure upgrade project.

**Manager:** What's happening?

**You:** The project requires 60 hours of work this week, but I'm the only one assigned. I also have BAU (business as usual) tasks that take 15-20 hours weekly.

**Manager:** So you're overallocated?

**You:** Yes, by about 35-40 hours. I can't complete both the project work and BAU responsibilities without help.

**Manager:** What do you need?

**You:** Either another team member to share the project work, or someone to cover my BAU tasks this week. The project has a hard deadline.

**Manager:** Let me check team availability. Can you break down which tasks could be delegated?

**You:** Sure. The monitoring setup and documentation could be delegated. The core infrastructure work needs my expertise.

**Manager:** Got it. I'll assign Tom to help with monitoring and documentation. That should free up 20 hours for you.

**You:** That would work perfectly. Thanks for the quick response!

**Manager:** No problem. Let's sync with Tom in 30 minutes to hand off the tasks.

---

### Conversation 5: Approval Blocker

**You:** Hi Rebecca, I have a blocker that needs your approval to resolve.

**Manager:** What is it?

**You:** The security scanning tool we need for the compliance audit costs $5,000 annually. I can't proceed with the implementation without budget approval.

**Manager:** Why do we need this specific tool?

**You:** The compliance requirements explicitly mandate this type of scanning. We evaluated 3 alternatives, and this is the most cost-effective option that meets all requirements.

**Manager:** What happens if we delay this purchase?

**You:** We miss the compliance audit deadline next month, which could result in penalties or losing our certification.

**Manager:** That's a clear business case. I'll approve it today. Send me the formal request with the vendor details.

**You:** Already prepared! I'll forward it to you in the next 5 minutes.

**Manager:** Perfect. Once I approve, how long until you can implement it?

**You:** The tool setup takes 2 days, then we need 1 week for scanning and remediation. So 9 days total.

**Manager:** That leaves us buffer time before the audit. Good planning.

---

### Conversation 6: Communication Blocker

**You:** I'm having trouble getting information I need from the development team.

**Scrum Master:** What information do you need?

**You:** I need the application's system requirements - CPU, memory, disk space. I've asked twice via email and Slack, but haven't received a response.

**Scrum Master:** When did you first ask?

**You:** Three days ago. I followed up yesterday but still no response. This blocks me from sizing the production servers correctly.

**Scrum Master:** That's too long. Let me help. Who specifically should provide this info?

**You:** The lead developer, John. I think he might be overwhelmed with the release preparation.

**Scrum Master:** I'll talk to him directly and set up a quick 15-minute call between you two this afternoon. Does 2 PM work?

**You:** Yes, perfect. I just need 10 minutes to get the specs.

**Scrum Master:** Great. I'll send the calendar invite and mark it as priority.

**You:** Thank you! This really helps.

---

### Conversation 7: Multiple Blockers

**Team Lead:** How's the deployment automation project going?

**You:** I need to discuss several blockers that are impacting progress.

**Team Lead:** Let's hear them all.

**You:** First blocker - we don't have a testing environment that matches production. This makes it risky to test automation scripts.

**Team Lead:** Okay, that's important. What else?

**You:** Second - the Jenkins plugin we need isn't approved by IT security yet. I submitted the request 2 weeks ago with no response.

**Team Lead:** Got it. Third blocker?

**You:** The API credentials for the deployment platform expired, and I need the vendor to regenerate them. They said 3-5 business days.

**Team Lead:** That's a lot. Let me prioritize these. The testing environment is highest priority - I'll work with the infrastructure team to clone production.

**You:** That would help immensely.

**Team Lead:** For the Jenkins plugin, I'll escalate to IT security today. For the API credentials, can you work on other parts of the automation while waiting?

**You:** Yes, I can work on the script logic and testing framework.

**Team Lead:** Good. Let's resolve these one by one. I'll send updates by end of day.

---

### Conversation 8: Blocker Resolution Follow-up

**[Slack Message]**

**You:** @Manager Quick update - the blocker from yesterday is now resolved! 🎉

**Manager:** Great! What was the resolution?

**You:** The network team opened the required firewall ports. I can now connect to the external API and continue with the integration.

**Manager:** How long did it take once they started?

**You:** Only 30 minutes. The actual fix was quick, just needed approval and scheduling.

**Manager:** Good. Are you back on track with the timeline?

**You:** Yes, I lost about 4 hours total yesterday, but I'm working extra today to make up for it. Should be caught up by end of day.

**Manager:** Appreciate the dedication. Let me know if you need anything else.

**You:** Will do, thanks for your help escalating this yesterday!

---

### Conversation 9: Preventing Future Blockers

**You:** I want to discuss how we can prevent blockers we've been experiencing.

**Team Lead:** Good idea. What are you thinking?

**You:** We've had 3 blockers this month related to access and permissions. Each one delayed work by 1-2 days.

**Team Lead:** I've noticed that too. What's your suggestion?

**You:** I propose we create a "new project checklist" that includes all access requirements upfront - database credentials, API keys, environment access, etc.

**Team Lead:** That makes sense. What else?

**You:** Also, for external dependencies like third-party APIs, we should identify backup options or mock services during the planning phase.

**Team Lead:** Both are good ideas. Can you draft the checklist?

**You:** Sure, I'll have it ready by tomorrow. I'll include all the blocker types we've encountered and how to prevent them.

**Team Lead:** Perfect. We'll review it in Friday's team meeting and make it part of our standard process.

**You:** Sounds good. This should save us a lot of frustration going forward.

---

### Conversation 10: Escalating a Critical Blocker

**You:** I need to escalate a critical blocker immediately.

**Manager:** What's wrong?

**You:** The production database is locked for maintenance by the DBA team, but they didn't inform us. We're supposed to deploy in 2 hours and can't proceed without database access.

**Manager:** Did you reach out to them?

**You:** Yes, I called and sent urgent messages. They say the maintenance will take another 3 hours. This will cause us to miss our deployment window.

**Manager:** That's a big problem. The client is expecting this deployment today.

**You:** Exactly. We have two options: Either the DBA team accelerates their maintenance, or we postpone the deployment to tomorrow's maintenance window.

**Manager:** What's your recommendation?

**You:** I recommend we postpone. Rushing the DBA team could lead to mistakes. A 24-hour delay is better than potential data corruption.

**Manager:** I agree, but I need to inform the client. Can you prepare a brief explanation of why this happened and how we'll prevent it?

**You:** Yes, I'll have that ready in 15 minutes. Going forward, I'll make sure we check the maintenance calendar before scheduling deployments.

**Manager:** Good. I'll handle the client communication. You coordinate the new deployment time with the DBA team.

**You:** On it. I'll send a revised deployment plan within the hour.

---