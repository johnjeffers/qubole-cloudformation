# qubole-cloudformation

A CloudFormation template for a Qubole bastion host, IAM role and policies, and RDS instance for metastore.

## TODO After Stack Creation

### Import the Metastore Schema

After creating the stack, import the schema to the metastore RDS instance created by the template. For example:

```
mysql -h qubole-metastore.a1s2d3f4g5.us-east-1.rds.amazonaws.com -u db_admin -p qubole < metastore.sql
```

You will then need to go to the Qubole UI -> Explore -> Hive Metastore and update the system to point to the custom metastore you just created. It should be fairly self-explanatory. Make sure to check both `Cluster Access` and `Use Bastion Node`.

### us.qubole.com Accounts Only<br>Add the Account's Public Key to the Bastion Host

1. Go to the Qubole console, and select the Account you are setting up.
2. Go to the dropdown menu and select My Accounts
3. Click Show in the API Token column. Copy the token.
4. Run this curl to retrieve the public key (update the token with your own)
   ```
   curl -X GET -H "X-AUTH-TOKEN:your_token" -H "Content-Type:application/json" -H "Accept: application/json" "https://us.qubole.com/api/v1.2/accounts/ssh_key"
   ```
5. Log on to the bastion host and add the key to `/home/ec2-user/.ssh/authorized_keys`. You may need to update the bastion host's Security Group to get access. Be sure to delete any custom SG rules you have added when you're done.