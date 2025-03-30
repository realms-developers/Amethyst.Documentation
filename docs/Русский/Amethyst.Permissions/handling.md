# Обработка прав

## Что такое PermissionAccess?

`PermissionAccess` - это `enum`, который используется для обработки результатов.

```cs
public enum PermissionAccess
{
    // пропускает проверку
    // в модуле Groups это возвращается, когда группа игрока не имеет право.
    None,

    // означает, что объект (допустим NetPlayer) может иметь право
    HasPermission,

    // означает, что объект (допустим NetPlayer) критически не должен иметь определенное право
    // используйте это редко, так как чрезмерное употребление этого результата, может сломать другие плагины и модули 
    Blocked
}
```

Допустим, нам нужно расширение прав для `NetPlayer`.

Нужно создать расширение прав для него:

```cs
public sealed class MyPermissionsWorker : IPermissionWorker<NetPlayer>
{
    public static List<string> DefaultPermissions = new List<string>()
    {
        "perm1", "perm2", "test"
    };

    public PermissionAccess HasPermission(NetPlayer target, string permission)
    {
        // если право "MyPermission", то это расширение полностью блокирует его
        // другие расширения не могут на это повлиять, так как это расширение полностью его заблокировало
        if (permission == "право_которое_игрок_фиксированно_не_должен_иметь")
            return PermissionAccess.Blocked;

        // если игрок имеет право, которое перечислено в DefaultPermissions, то расширение возвращает PermissionAccess.HasPermission.
        if (DefaultPermissions.Contains(permission))
            return PermissionAccess.HasPermission;

        // расширение игнорирует проверку, так как право не сошлось ни с чем.
        return PermissionAccess.None;
    }

    public PermissionAccess HasChestPermission(NetPlayer target, int x, int y)
    {
        // расширение игнорирует проверку
        return PermissionAccess.None;
    }

    public PermissionAccess HasChestEditPermission(NetPlayer target, int x, int y)
    {
        // расширение игнорирует проверку
        return PermissionAccess.None;
    }

    public PermissionAccess HasSignPermission(NetPlayer target, int x, int y)
    {
        // расширение игнорирует проверку
        return PermissionAccess.None;
    }

    public PermissionAccess HasSignEditPermission(NetPlayer target, int x, int y)
    {
        // расширение игнорирует проверку
        return PermissionAccess.None;
    }

    public PermissionAccess HasTEPermission(NetPlayer target, int x, int y)
    {
        // расширение игнорирует проверку
        return PermissionAccess.None;
    }

    public PermissionAccess HasTilePermission(NetPlayer target, int x, int y, int? width = null, int? height = null)
    {
        // расширение игнорирует проверку
        return PermissionAccess.None;
    }
}
```

Теперь, что бы расширение работало, нужно его зарегистрировать в узле прав (`PermissionNode<T>`).

В случае с `NetPlayer` - это `AmethystSession.PlayerPermissions`.

```cs
IPermissionWorker<NetPlayer> worker = new MyPermissionsWorker();

// регистрация
AmethystSession.PlayerPermissions.Register(worker);

// дерегистрация
AmethystSession.PlayerPermissions.Unregister(worker);
```

Обязательно дерегистрируйте расширение прав, если оно из плагина.