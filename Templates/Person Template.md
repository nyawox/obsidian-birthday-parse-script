---
name: <% tp.file.title %>
pronouns:
relationship:
birthday: <% tp.date.now("YYYY-MM-DD") %>
email:
phone:
socialprofiles:
 -
tags: 
 - Person
---

# `= this.name`
## Information
- **Pronouns**: `= this.pronouns`
- **Relationship**: `= this.relationship`
- **Age**: `= floor((date(today) - date(this.birthday)).years)`
## Contacts
- **Email**: `= this.email`
- **Phone**: `= this.phone`
```dataviewjs 
const socialProfiles = dv.current().socialprofiles;
try {
  for (const profile of socialProfiles) {
    const [platform, handle] = profile.split(' ', 2);     
    dv.paragraph(`- **${platform}**: ${handle}`);   
  }
} catch (error) {}
```
## Notes
- 
## Meetings
- 