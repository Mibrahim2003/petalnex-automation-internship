const rows = $input.all().map((item) => item.json);

const toTitleCase = (value) =>
  (value ?? '')
    .trim()
    .replace(/\s+/g, ' ')
    .split(' ')
    .map((word) =>
      word ? word.charAt(0).toUpperCase() + word.slice(1).toLowerCase() : word
    )
    .join(' ');


const toGrade = (score) => {
  if (score >= 90) return 'A';
  if (score >= 80) return 'B';
  if (score >= 70) return 'C';
  if (score >= 60) return 'D';
  return 'F';
};

const passingGrades = ['A', 'B'];

const cleaned = rows.map((r) => ({
  name: toTitleCase(r.name),
  email: (r.email ?? '').trim().toLowerCase(),
  score: r.score,
  grade: toGrade(r.score),
}));

const passed = cleaned.filter((r) => passingGrades.includes(r.grade));

return passed.map((r) => ({ json: r }));