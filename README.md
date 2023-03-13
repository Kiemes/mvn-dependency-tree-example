This simple project shows the issue in maven dependency plugin < 3.3.0 using the `verbose` option.

Running `mvn org.apache.maven.plugins:maven-dependency-plugin:2.8:tree -Dverbose` generates different output compared to
`mvn org.apache.maven.plugins:maven-dependency-plugin:3.5.0:tree -Dverbose`

The `verbose` parameter was not supported in version [3.0.0,3.3.0).

The 2.8 with `verbose` option contained `snakeyaml` which it does not contain without the `verbose` option.
However, snakeyaml was never part of the dependencies since its explicitly excluded.  
The output of 2.8 with verbose is not correct.  
The output of 2.8 without verbose was correct.  
The output of 3.5 is correct with or without verbose.

The unified-agent.jar should switch to 3.5.0 without `verbose` option.
The verbose option shows "omitted nodes in the serialized dependency tree". Since the omitted nodes will not be part of the resulting build artifact those lines of the tree will not be a security issue.
Hence, `verbose` should not be used by unified-agent.jar.

Some links:
https://maven.apache.org/plugins/maven-dependency-plugin/tree-mojo.html#verbose
https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12352602&styleName=&projectId=12317227&Create=Create&atl_token=A5KQ-2QAV-T4JA-FDED_e13a5233cd035e0af4acaeb2600d478c0571f336_lout
