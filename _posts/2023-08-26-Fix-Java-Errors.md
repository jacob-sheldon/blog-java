---
layout: post
title:  "Fix Java Exceptions"
date:   2023-08-26 19:03:07 +0800
categories: Hand On
# permalink: fix-java-errors
---
# Fix Java Exceptions

## Repository

### org.hibernate.PersistentObjectException: detached entity passed to persist:

```
class InterviewPeriod {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Integer id;
}
```

```
var interivewPeriod = InterviewPeriod.builder()
        .id(1)
        .build();
entityManager.persist(interivewPeriod);
```

If you set the `id` field like this, and use `TestEntityManager` to persit, the error will appear.

To fix it, just don't set `id` manually.
