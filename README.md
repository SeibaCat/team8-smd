# FlexPay: flexible compensation for Spanish companies

Team 8 group project for **Software Management and Development** (IE University).

## Theme

In Spain, employees can receive part of their salary as benefits: a meal card, a transport pass, health insurance, childcare or training. Up to legal limits, that money is **not taxed**. An employee who earns €2,500 a month and puts €200 into benefits pays income tax on €2,300 only: the total salary stays the same and they take home more.

**FlexPay** lets a company:

- manage its employees and the benefits each one chooses;
- record what they spend on each benefit;
- calculate payroll with the tax savings applied;
- show a dashboard with how much each employee, each department and the whole company saves.

## Team, modules and shared jobs

| Member | GitHub | Module | Shared job |
|---|---|---|---|
| Nicolas Montero | [@SeibaCat](https://github.com/SeibaCat) | 6 · Savings & budgets report | Frontend layout and dashboard |
| Alifa Hasnanda | [@alifahasnanda](https://github.com/alifahasnanda) | 1 · Employees | README, architecture diagram, project board and code review rules |
| Pablo Chivite | [@pablochivite](https://github.com/pablochivite) | 5 · Payroll | Continuous integration (GitHub Actions) |
| Raul Gurdian | [@raulgurdian](https://github.com/raulgurdian) | 2 · Benefits catalogue | Database setup, shared rules and sample data |
| Eric Ulgen | [@ericgames234](https://github.com/ericgames234) | 4 · Benefit spending | Docker and configuration |
| Leonid Fedorov | [@leofedie](https://github.com/leofedie) | 3 · Benefit plans | Azure deployment |

## Modules

Each module has its own database table, API router under `/api/<module>/`, page in the frontend, tests, and at least one calculated result. **Every module can be built on its own**: it stores the inputs it needs and uses the shared rules below, so nobody waits for another member's code to build, test or merge their module.

| # | Module | Main entity | Calculated result (works alone) | Connects to (Milestone 3) | Issue |
|---|---|---|---|---|---|
| 1 | Employees | Employee (and user): name, department, gross annual salary | Total cost per employee: salary + employer social security (flat 30 %) | Every module uses its `employee_id` | #1 |
| 2 | Benefits catalogue | Benefit offered by the company: code, provider, monthly price | Tax-free and taxable part of a benefit for a given amount | Plans and Spending read the catalogue | #2 |
| 3 | Benefit plans | BenefitPlan: `employee_id`, benefit code, monthly amount | Check against the caps and the 30 % limit; amount moved out of the taxable salary | Payroll reads the plan total | #3 |
| 4 | Benefit spending | Transaction: `employee_id`, benefit code, date, merchant, amount | Split of spending into tax-free and taxable, per month | The dashboard reads the totals | #4 |
| 5 | Payroll | Payslip: `employee_id`, month, gross salary, flexible amount | Gross → taxable base → income tax → net pay | Salary from Employees, flexible amount from Plans | #5 |
| 6 | Savings & budgets report | SavingsScenario (salary, flexible amount) and DepartmentBudget | Tax with the plan vs without it; budget vs actual | The dashboard combines Employees, Plans, Spending and Payroll | #6 |

**Cross-module feature:** at Milestone 3, Payroll takes the salary from Employees and the flexible amount from Benefit plans instead of storing its own copy, and the dashboard combines all modules into the savings per employee, per department and for the whole company.

## Working independently

- **One folder, one router, one table per module.** You only edit your own module's files, so two people never change the same file.
- **Links are IDs.** Modules refer to an employee with an integer `employee_id` and to a benefit with its code: `meal`, `transport`, `health`, `childcare`, `training`. Foreign keys and calls between modules are added at Milestone 3, when the modules are put together; the copies a module keeps until then (like the salary in a payslip) are replaced by the real data.
- **Shared rules in one place.** The values below go in a single shared `rules.py`, added in the first pull request of Milestone 2 (Raul). Every module imports them from there; nobody copies them.
- **Your own sample data.** Tests and pages use the module's own sample rows, so no module waits for another to be finished.
- **Money:** `Decimal` in Python, whole cents (or `NUMERIC`) in the database, never a float. **Dates:** ISO `YYYY-MM-DD`.

## Shared rules (simplified)

Tax-free limit of each benefit (Spanish IRPF, simplified for this project):

| Code | Benefit | Tax-free limit |
|---|---|---|
| `meal` | Meal card | €11 per working day (we assume 20 working days a month: €220) |
| `transport` | Public transport | €136.36 per month, €1,500 per year |
| `health` | Health insurance | €500 per insured person per year |
| `childcare` | Childcare | No limit |
| `training` | Job-related training | No limit |

All benefits together can't exceed **30 % of the gross annual salary**.

Income tax (IRPF), simplified: one national scale, no regions or personal allowances, applied to the yearly taxable base:

| Taxable base (€/year) | Rate |
|---|---|
| 0 – 12,450 | 19 % |
| 12,450 – 20,200 | 24 % |
| 20,200 – 35,200 | 30 % |
| 35,200 – 60,000 | 37 % |
| 60,000 – 300,000 | 45 % |
| over 300,000 | 47 % |

## Project board

Tasks are GitHub issues on the [Team 8 board](https://github.com/users/SeibaCat/projects/1), one issue per module, each assigned to its owner.
