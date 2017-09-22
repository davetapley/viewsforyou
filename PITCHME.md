# ~~Vue~~ View

---

# View (SQL)

> In database theory, a view is the result set of a stored query on the
> data, which the database users can query just as they would in a
> persistent database collection object.

---

```sql
CREATE TABLE cards (
    id int,
    supplier_id :: int,
    created_at :: timestamp,
    balance_cents :: int
);
```

---

```sql
CREATE TABLE cards (
    id int,
    supplier_id :: int,
    created_at :: timestamp,
    balance_cents :: int
);
```

```sql
CREATE VIEW bills AS
  SELECT
      supplier_id,
      (created_at :: date) AS date,
      sum(balance_cents) AS total_cents

  FROM cards

  GROUP BY
    supplier_id,
    (created_at :: date)
;
```
---

Days where someone sold us over $100 of cards:

```sql
SELECT date, total_cents
FROM bills
WHERE supplier_id = 123
AND total_cents > 10000
ORDER BY date;
```

Didn't even mention the `cards` table, neat.

---

ORM MY GOD

It's full of objects:

```ruby
class Bill < ActiveRecord::Base
  belongs_to :supplier

  def readonly?
    true
  end
end
```

---

Sure, you could...

```ruby
class Card < ActiveRecord::Base
  after_create :update_bills

  def update_bills
    bill = Bill.find_or_initialize_by(
      supplier_id: supplier_id,
      date: created_on.to_date
    )

    bill.total_cents += balance_cents
    bill.save
  end
end
```

---

# So why?

1. Because the view approach is better
1. Because opinions


---

View is **declarative**:
> expresses the logic of a computation without describing its control flow

vs

Callback is **imperative**:
> uses statements that change a program's state

---

# So why?

1. Because view is better
1. Because ~~opinion~~ **declarative** code is better

---

# So why?

1. Because view is better
1. Because **declarative** code is better
1. Because there's less for me to get wrong?

---

# Prefer to be declarative

An opinion which is at least **humble**.

---

# Dislaimer 1

I get very excited about functional programming languages.

_When I grow up I want to write Haskell_.

---

# Disclaimer 2

We can talk about performance later.

Yes, it's totally a thing.

---

EOF
