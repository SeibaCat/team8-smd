# FlexPay: flexible compensation for Spanish companies

Team 8 group project for **Software Management and Development** (IE University).

## Theme

In Spain, employees can receive part of their salary as benefits: a meal card, a transport pass, health insurance, childcare or training. Up to legal limits, that money is **not taxed**. An employee who earns €2,500 a month and puts €200 into benefits pays income tax on €2,300 only: the total salary stays the same and they take home more.

**FlexPay** lets a company:

- manage its employees and the benefits each one chooses;
- record what they spend on each benefit;
- calculate payroll with the tax savings applied;
- show a dashboard with how much each employee, each department and the whole company saves.

**Cross-module feature:** Payroll uses Employees and Benefit plans to compute the taxable base, and the Savings & budgets dashboard combines Employees, Benefit plans, Benefit spending and Payroll.

## Team, modules and shared jobs

| Member | GitHub | Module | Shared job |
|---|---|---|---|
| Nicolas Montero | [@SeibaCat](https://github.com/SeibaCat) | 6 · Savings & budgets report | Frontend layout and dashboard |
| Alifa Hasnanda | [@alifahasnanda](https://github.com/alifahasnanda) | 1 · Employees | README, architecture diagram, project board and code review rules |
| Pablo Chivite | [@pablochivite](https://github.com/pablochivite) | 5 · Payroll | Continuous integration (GitHub Actions) |
| Raul Gurdian | [@raulgurdian](https://github.com/raulgurdian) | 2 · Benefits catalogue | Database setup and sample data |
| Eric Ulgen | [@ericgames234](https://github.com/ericgames234) | 4 · Benefit spending | Docker and configuration |
| Leonid Fedorov | [@leofedie](https://github.com/leofedie) | 3 · Benefit plans | Azure deployment |

## Modules

Each module has its own database table, API router under `/api/<module>/`, page in the frontend, tests, and at least one calculated result.

| # | Module | Main entity | Calculated result | Issue |
|---|---|---|---|---|
| 1 | Employees | Employee (and user) | Total cost per employee | #1 |
| 2 | Benefits catalogue | Benefit (meal, transport, health insurance, childcare, training) with its tax-free limit | Remaining tax-free allowance of a benefit | #2 |
| 3 | Benefit plans | BenefitPlan: what each employee chooses | Validation against the legal caps; total moved out of the taxable salary | #3 |
| 4 | Benefit spending | Transaction made with the benefits card | Split of spending into tax-free and taxable | #4 |
| 5 | Payroll | Payslip | Gross salary → taxable base → income tax → net pay | #5 |
| 6 | Savings & budgets report | DepartmentBudget | Tax with the plan vs without it, per employee and department; budget vs actual spending | #6 |

Amounts of money are `Decimal` in Python and whole cents (or `NUMERIC`) in the database, never a float.

## Project board

Tasks are GitHub issues on the [Team 8 board](https://github.com/users/SeibaCat/projects/1), one issue per module, each assigned to its owner.
