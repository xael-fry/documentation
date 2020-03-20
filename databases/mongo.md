
# Dump mongo database
```
mongodump --username nex --password mdp4nex --authenticationDatabase admin --db next_ci --out next_ci
```


# Restore  mongo database

```
mongorestore --username nex --password mdp4nex --authenticationDatabase admin next_live
```

# Connect to mongo database

```
mongo -u nex -p mdp4nex --authenticationDatabase admin next_live
```

