---
created: <% tp.file.creation_date() %>
---
tags:: [[+Daily Notes]]

# <% moment(tp.file.title,'YYYY-MM-DD').format("dddd, MMMM DD, YYYY") %>

<< [[Timestamps/<% tp.date.now("YYYY", -1) %>/<% tp.date.now("MM-MMMM", -1) %>/<% tp.date.now("YYYY-MM-DD-dddd", -1) %>|Yesterday]] | [[Timestamps/<% tp.date.now("YYYY", 1) %>/<% tp.date.now("MM-MMMM", 1) %>/<% tp.date.now("YYYY-MM-DD-dddd", 1) %>|Tomorrow]] >>

---

### Daily Todos

- [ ] Practice [[Gateway Process/Focus 10|Focus 10]]
- [ ] Yoga/exercise
- [ ] Walk 
- [ ] Read for 10 minutes 
### Daily Questions

#####  One memory from my meditation..  
- 
##### Some things I want to do for date night...
- [[Date Night Ideas]]
##### One thing that is important to me ...
- 
##### Something about circling today...  
- 
##### One+ thing I accomplished today is...
- [ ] 
##### One thing I'm struggling with today is...
- 

---
# 📝 Notes
Today I am celebrating 

Struggle I'm having 

Looking forward to 

---
### Notes created today
```dataview
List FROM "" WHERE file.cday = date("<%tp.date.now("YYYY-MM-DD")%>") SORT file.ctime asc
```

### Notes last touched today
```dataview
List FROM "" WHERE file.mday = date("<%tp.date.now("YYYY-MM-DD")%>") SORT file.mtime asc
```