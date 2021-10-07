# A problem of maybes

The life is full of maybes. So are some problems that we solve in Tech.

Lets suppose we receives a given json:

```json

{
  "org": "Konoha",
  "full_name": "Konohagakure no Sato",
  "statistics": {
    "population": "5/5",
    "military": "3/5",
    "economy": "2/5"
  },
  "divisions": [
    {
    "type": "MEDIC",
    "member_count": 50,
    "leader": "Sakura Haruno",
    "leader_id": 489
    },
    {
      "type": "ANBU",
      "member_count": 30,
      "leader": "Hatake Kakashi",
      "leader_id": 340
    },
    {
      "type": "ROOT",
      "member_count": 10,
      "leader": "Shimura Danzō",
      "leader_id": 130
    },
  ]
}
```

But now root division has been merged in to Anbu branch, but you have to change this data before you can show it your User Interface. 

Oh, Danzõ is dead. So dont forget add the remaining members of root into the member count of Anbu.

Important techinical detail: Our backend that serves this json don't make garantees abou the order of the `divisions` list.
