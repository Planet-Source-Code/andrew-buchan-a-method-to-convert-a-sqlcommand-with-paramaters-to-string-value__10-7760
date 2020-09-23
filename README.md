<div align="center">

## A Method to convert a SqlCommand with paramaters to string value


</div>

### Description

This code converts SqlParameters of a SqlCommand object into their literal value.

For instance, if you have a SqlCommand which runs a query which has parameters, you will never actually see the query that your database engine runs.

Using this method, the SqlCommand query is converted so that you see exactly what is run on your database engine.

This works with SqlParameters of type VarChar, Bit and Int, however all are supported in some way.
 
### More Info
 
SqlCommand object

The SqlCommand must be initialsed before use.

To call this method:

SqlCommand cmd = new SqlCommand("SELECT Col1, Col2, Col3 FROM TableName WHERE (Col1 = @Prm1) AND (Col2 = @Prm2) AND (Col3 = @Prm3)");

cmd.Parameters.Add("@Prm1", SqlDbType.VarChar).Value = "000";

cmd.Parameters.Add("@Prm2", SqlDbType.Int).Value = 500;

cmd.Parameters.Add("@Prm3", SqlDbType.Bit).Value = false;

ConvertCommandParamatersToLiteralValues(cmd);

String version of the input SqlCommand with its parameters values


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Andrew Buchan](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/andrew-buchan.md)
**Level**          |Intermediate
**User Rating**    |4.0 (8 globes from 2 users)
**Compatibility**  |C\#
**Category**       |[Databases](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/databases__10-5.md)
**World**          |[\.Net \(C\#, VB\.net\)](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/net-c-vb-net.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/andrew-buchan-a-method-to-convert-a-sqlcommand-with-paramaters-to-string-value__10-7760/archive/master.zip)

### API Declarations

Copyright Andrew Buchan, www.andrewbuchan.co.uk 2010


### Source Code

```
private static string ConvertCommandParamatersToLiteralValues(SqlCommand cmd)
{
  string query = cmd.CommandText;
  foreach (SqlParameter prm in cmd.Parameters)
  {
    switch (prm.SqlDbType)
    {
      case SqlDbType.Bit:
        int boolToInt = (bool)prm.Value ? 1 : 0;
        query = query.Replace(prm.ParameterName, string.Format("{0}", (bool)prm.Value ? 1 : 0));
        break;
      case SqlDbType.Int:
        query = query.Replace(prm.ParameterName, string.Format("{0}", prm.Value));
        break;
      case SqlDbType.VarChar:
        query = query.Replace(prm.ParameterName, string.Format("'{0}'", prm.Value));
        break;
      default:
        query = query.Replace(prm.ParameterName, string.Format("'{0}'", prm.Value));
        break;
    }
  }
  return query;
}
```

