# 🧪 Cross-User Party System - Debugging Guide

## 🔍 **How to Debug the "Can't Send Messages" Issue**

### **Step 1: Enable Console Logging**
1. Open your browser's Developer Tools (F12)
2. Go to the "Console" tab
3. Reproduce the issue (try to send a message as a participant)

### **Step 2: Look for These Key Log Messages**

#### **When Joining as Participant:**
```
=== JOIN PARTY DEBUG INFO ===
Current user: ParticipantName
Current party members: [HostName]
Party host: HostName
Is host: false
➕ Adding ParticipantName to party members
Updating party data with new member...
=== WRITE PARTY DATA ATTEMPT ===
[ParticipantName] User is PARTICIPANT for party ABC123
⏭️ Skipping hosting update - user is not host for party ABC123
📁 Updating file backup for party ABC123
📁 Party ABC123 stored as backup file (attempt 1)
```

#### **When Sending Message as Participant:**
```
=== MESSAGE SEND ATTEMPT ===
[ParticipantName] Sending message: Hello!
Getting latest party data before sending message...
=== READ PARTY DATA ATTEMPT ===
Trying hosting endpoint: https://party-data-abc123.puter.site/data.json
✅ Found party ABC123 via HOSTING (cross-user)
Latest party data retrieved: {members: ["HostName", "ParticipantName"], messages: [...]}
✅ User ParticipantName confirmed as party member
Writing updated party data with new message...
=== WRITE PARTY DATA ATTEMPT ===
[ParticipantName] User is PARTICIPANT for party ABC123
⏭️ Skipping hosting update - user is not host for party ABC123
📁 Updating file backup for party ABC123
📁 Party ABC123 stored as backup file (attempt 1)
✅ [ParticipantName] Party ABC123 stored successfully for cross-user access
✅ Message successfully sent by ParticipantName
```

### **Step 3: Common Issues to Look For**

#### **❌ Permission Denied Errors:**
```
❌ Hosting setup error for party ABC123: Permission denied
```
**Fix:** This should be resolved - participants now skip hosting updates

#### **❌ Party Not Found:**
```
❌ Party ABC123 not found in any storage method
```
**Fix:** Wait longer for hosting propagation (30-60 seconds)

#### **❌ User Not in Member List:**
```
❌ User ParticipantName not in party members and outside grace period
```
**Fix:** This should be resolved with the extended grace period

### **Step 4: Manual Testing**

#### **Test Hosting Endpoint:**
1. Open a new browser tab
2. Go to: `https://party-data-[PARTYCODE].puter.site/data.json`
3. You should see JSON data with members and messages

#### **Test File Access:**
1. In browser console, run:
```javascript
puter.fs.read('party_ABC123.json').then(blob => blob.text()).then(console.log)
```

### **Step 5: If Still Not Working**

1. **Clear browser data** and refresh both host and participant browsers
2. **Wait longer** for hosting propagation (up to 60 seconds)
3. **Check party code** is exactly correct (case-sensitive)
4. **Verify both users** are signed into Puter

### **Expected Behavior**

✅ **Host** can send messages → Updates hosting + backups  
✅ **Participant** can send messages → Updates backups only  
✅ **Everyone** can read messages → From hosting endpoint  
✅ **No permission errors** → Participants skip hosting updates  

The system is designed to work even if participants can't update the hosting endpoint!