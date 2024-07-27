<%*
const peopleNotes = await app.vault.getMarkdownFiles();
const ensureDirectoryExists = async (dir) => {
  const abstractDir = app.vault.getAbstractFileByPath(dir);
  if (!abstractDir) {
    await app.vault.createFolder(dir);
  }
};
const birthdaysDir = "Birthdays";
await ensureDirectoryExists(birthdaysDir);
for (const file of peopleNotes) {
  const content = await app.vault.read(file);
  const frontmatter = app.metadataCache.getFileCache(file).frontmatter;
  const fileName = file.basename;

  // Exclude the person template file
  if (fileName === "Person Template") {
    continue;
  }

  if (frontmatter && frontmatter.birthday) {
    const birthday = frontmatter.birthday;
    const name = frontmatter.name;
    const birthDate = new Date(birthday);
    const ageDifMs = Date.now() - birthDate.getTime();
    const ageDate = new Date(ageDifMs);
    const age = Math.abs(ageDate.getUTCFullYear() - 1970);
    const noteContent = `---
type: rrule
allDay: true
startDate: ${birthday}
rrule: FREQ=YEARLY
skipDates: []
title: ${name}'s Birthday
tags:
 - BirthdayNotes
---
[[${name}]]'s Birthday event note for full calendar plugin
Auto generated via script
`;
    const filePath = `Birthdays/${birthday} ${name}'s Birthday.md`;
    const existingFile = app.vault.getAbstractFileByPath(filePath);
    if (existingFile) {
      await app.vault.modify(existingFile, noteContent);
    } else {
      await app.vault.create(filePath, noteContent);
    }
  }
}
%>