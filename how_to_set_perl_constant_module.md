# ������ŵ�ʹ��Perl�ĳ���ģ��

�����Perl Coding������һ�����⣺��Ҫ��һϵ�г������кϷ��Լ�⡣

��Research&Develop����һЩ�ĵá�

���Ĵ�Perl�ĳ��������ϣ�����һ��_����Ϊ_���ŵĽ��������

## Perl��һ��ĳ�������

��д��Ŀ��ʱ��Ϊ�˱���Magic Number����������Ǿ�����Ҫ���峣����

��Ȼ�ˣ�����[���д���޷�ά���Ĵ���][1]��ָ�����ǲ�Ӧ�����峣����NumberԽMagicԽ�á�

һ����˵��Perl�еĳ������£�

```perl
    use constant CONST_PI => 3.1416;
```

��ģ�黯�ı�������У����ų��������࣬���ǻ���Ҫ����Щ�����ŵ�һ������ģ��(Perl Module)���棺

����������Ҳ����DRY(Don't Repeat Yourself)��ԭ��#DRYBestPractice?

����˵�ں�������д��һ�����Ժ������������������ģ�顣

```perl
    package Lirian::Constants;
    use strict;
    use warnings;

    #Math Const
    use constant CONST_PI => 3.1416;
    use constant CONST_E  => 2.718;

    #Default Config Parm Name
    use constant PARM_DB_NAME  => 'dbName';
    use constant PARM_DB_HOST  => 'dbHost';
    use constant PARM_GIT_USER => 'gitUser';

    require Exporter;
    our @ISA = qw(Exporter);

    our @EXPORT_OK = qw(
        CONST_PI
        CONST_E
        PARM_DB_NAME
        PARM_DB_HOST
        PARM_GIT_USER
    );

    our %EXPORT_TAGS = (
        all  => \@EXPORT_OK,
        math => [qw(
            CONST_PI
            CONST_E
        )],
        parm => [qw(
            PARM_DB_NAME
            PARM_DB_HOST
            PARM_GIT_USER
        )],
    );
```

������δ���򵥴ֱ���չʾ��Perl�еĳ���ģ�顣

���е�`our @ISA = qw(Exporter);`�Ǳ�ʾ��ģ��̳���[Exporter][2]���ģ�顣

ͨ�������Ķ��壬���Ǳ�����һ�������ġ����Ĳֿ⡱�ˡ�

������ôʹ�ã����Ǿ�������һ�ڡ�


## ʹ�ó���ģ��

����һ���У�ͨ����Exporter�ļ̳У���������һ��Lirian::Constantsģ�顣

��һ��أ����ǻ���ôʹ�ã�

```perl
    package Lirian::Math;

    use Lirian::Constants qw(:all);
    use strict;
    use warnings;

    sub get_circum {
        my ($r) = @_;

        return 2 * CONST_PI * $r;
    }

    1;
```

������������£�������ʹ�÷����Ƚϼ򵥣�������������Щ�ƣ�

�ڳ���Ŀ�ʼʱ��������Ҫ�����û��������ļ���ȷ��ÿ����Ŀ�û�������Ч�ġ�

����˵�����ļ����£�

```config
    dbName=github
    dbHost=localhost
    gitUser=LKI
```

��Ȼ�أ�һ�ּ򵥵���΢���˵�ķ�������ô����

```perl
    sub validate_config_dumb {
        my ($conf) = @_;

        validate_parm($conf, PARM_DB_NAME);
        validate_parm($conf, PARM_DB_HOST);
        validate_parm($conf, PARM_GIT_USER);
    }
```

�������м������⣺

1.config parameterһ�࣬����ͺܳ�����

2.����config constantʱ������Ҫ�ĳ���ģ�飬����ҲҪ�ӣ���

3.�����Ĵ����ύ��ȥ�о��ö�������

4.����������[ײ���˱��][3]�ﶼ��������

����ô�������������ŵ������أ�


## ����Perl��Export����

��Exporter�У�������%EXPORT_TAGS�е��ļ�����ֱ�����ã�

```perl
    my $tags      = \$Lirian::Constants::EXPORT_TAGS;
    my $math_tags = $tags->{math}; # ['CONST_PI', 'CONST_E'];
    my $parm_tags = $tags->{parm}; # ['PARM_DB_NAME', 'PARM_DB_HOST', 'PARM_GIT_USER'];
```

�����Ϳ����õ������������ˣ���ʱ���Ǳ���Ҫ[��Perl�и��ݱ�������ȡ����ֵ][4]

```perl
    my $name  = 'CONST_PI';
    my $value = Lirian::Constants->$name; # 3.1416
```

��������һС�ڵ������У����ǿ��Եó�һ�������ŵĽ��������~

```perl
    sub validate_config {
        my ($conf) = @_;

        my $tags      = \$Lirian::Constants::EXPORT_TAGS;
        my $parm_tags = $tags->{parm};
        foreach my $parm_name (@$parm_tags) {
            my $parm_value = Lirian::Constants->$parm_name;
            validate_parm($conf, $parm_value);
        }
    }
```

�󹦸�ɣ��Ӵ��Ժ��ڼӳ�����ͬʱ��validate_config���Զ��ؼ�������������ѡ����~

��ʱ�����ʹ��[һ�����ŵ�Git Commit Message][5]���ύ��������:wink: 



[1]:http://coolshell.cn/articles/4758.html
[2]:http://perldoc.perl.org/Exporter.html
[3]:http://coolshell.cn/articles/2058.html
[4]:http://stackoverflow.com/questions/2187682/how-do-i-access-a-constant-in-perl-whose-name-is-contained-in-a-variable
[5]:http://whatthecommit.com/
