Postmortem: The Great Database Lockdown of 2024

Imagine you’re at a busy supermarket checkout, and the cashier suddenly decides to recount all the money in the till while you and everyone else wait impatiently. Now, imagine that cashier is our database, and the till is... well, everything. Welcome to our latest adventure!

Issue Summary:

Duration: 2 excruciating hours, from 10:00 AM to 12:00 PM WAT on August 15, 2024.
Impact: Our e-commerce platform slowed to a crawl. Page load times skyrocketed, and 70% of our users experienced the digital equivalent of being stuck in a traffic jam during rush hour. Many couldn’t complete purchases—talk about a sale that never happened!
Root Cause: A rogue batch update process decided to play Monopoly with our database tables, locking them up tighter than Fort Knox. The result? A queue of queries longer than the line at a Black Friday sale.

(Okay, this isn’t the actual diagram, but you get the idea!)

Timeline:

10:00 AM: The monitoring system lets out a scream (an alert), and our on-call engineer nearly spills their coffee noticing the spike in response times.
10:05 AM: Investigation kicks off. First suspect: a network gremlin causing trouble (because it’s always the network, right?).
10:15 AM: Network check complete. No gremlins found. Database team, you’re up!
10:25 AM: Database logs show high CPU usage and a pile-up of waiting queries. Things are heating up—literally.
10:40 AM: Initial suspect: a sneaky database query that might’ve regressed. (Spoiler: It’s a red herring.)
11:00 AM: False alarm! Code review clears the query of all charges. Time to take another look at the database metrics.
11:15 AM: Aha! Database lock contention spotted, with a batch update process as the culprit. Time to bring in the big guns.
11:25 AM: Batch process terminated. The locks are freed, and our database is breathing again.
12:00 PM: Services fully restored, and the team exhales for the first time in two hours.
Root Cause and Resolution:

Root Cause: The trouble started when a batch update process began to update pricing across the board. But instead of playing nice, it decided to lock up key tables, causing a massive traffic jam. Other queries couldn’t get through, and soon enough, our database was gasping for air.

Resolution: We pulled the plug on the offending batch process, which immediately released the locks. After ensuring no data had been corrupted, we kept a close watch on the system as it limped back to life, eventually sprinting as normal.

Corrective and Preventative Measures:

Improvements:

Schedule batch processes like a responsible adult—no more rush-hour updates!
Beef up our lock contention monitoring so we catch these issues before they turn into full-blown crises.
More cross-team pow-wows to ensure everyone knows when big, potentially disruptive changes are happening.
Tasks:

 Set up alerts for lock contention so we can catch them early.
 Refactor batch update processes to be more considerate (i.e., use smaller transactions or break them into chunks).
 Hold a team training session on database best practices—bring snacks, make it fun!
 Update our incident response checklist to include steps for dealing with lock-related issues.
Remember: In the world of databases, it’s better to ask for permission (or a lock timeout) than forgiveness. Stay vigilant, and let’s avoid another episode of the Great Database Lockdown!
