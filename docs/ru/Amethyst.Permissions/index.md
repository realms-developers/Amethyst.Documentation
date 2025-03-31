# Интерфейсы прав

Amethyst предоставляет возможность плагинам и модулям управлять правами игроков через интерфейсы `IPermissionWorker<T>` и `PermissionsNode<T>`.

Интерфейс `IPermissionable` - обертка прав для объекта, `PermissionNode<T>` - узел, который используется для проверки прав между расширениями.

`IPermissionWorker<T>` - расширение прав для `IPermissionable`

## Что такое `IPermissionable`?

`IPermissionable` - интерфейс, который обязательно должен присутствовать у всех объектов, которые должны работать по системе прав.

Этому пример: `NetPlayer`, `ConsoleSender`

Рассмотрим класс `NetPlayer` и как он работает с правами:

```cs
public sealed class NetPlayer : IPermissionable
{
    public bool HasPermission(string permission)
        => IsCapable && (IsRootGranted || AmethystSession.PlayerPermissions.HandleResult(p => p.HasPermission(this, permission)) == PermissionAccess.HasPermission);

    public bool HasChestPermission(int x, int y)
        => IsCapable && (IsRootGranted || AmethystSession.PlayerPermissions.HandleResult(p => p.HasChestPermission(this, x, y)) == PermissionAccess.HasPermission);

    public bool HasChestEditPermission(int x, int y)
        => IsCapable && (IsRootGranted || AmethystSession.PlayerPermissions.HandleResult(p => p.HasChestEditPermission(this, x, y)) == PermissionAccess.HasPermission);

    public bool HasSignPermission(int x, int y)
        => IsCapable && (IsRootGranted || AmethystSession.PlayerPermissions.HandleResult(p => p.HasSignPermission(this, x, y)) == PermissionAccess.HasPermission);

    public bool HasSignEditPermission(int x, int y)
        => IsCapable && (IsRootGranted || AmethystSession.PlayerPermissions.HandleResult(p => p.HasSignEditPermission(this, x, y)) == PermissionAccess.HasPermission);

    public bool HasTEPermission(int x, int y)
        => IsCapable && (IsRootGranted || AmethystSession.PlayerPermissions.HandleResult(p => p.HasTEPermission(this, x, y)) == PermissionAccess.HasPermission);

    public bool HasTilePermission(int x, int y, int? width = null, int? height = null)
        => IsCapable && (IsRootGranted || AmethystSession.PlayerPermissions.HandleResult(p => p.HasTilePermission(this, x, y, width, height)) == PermissionAccess.HasPermission);
}
```

Игрок является всеправным при включенном `IsRootGranted` (команда `/grantroot` в режиме отладки).

Но основная часть ложится на `PermissionNode<NetPlayer>`, экземпляр которого находится в `AmethystSession.PlayerPermissions`.

Часть исходного кода `PermissionNode<T>`:

```cs
public class PermissionsNode<T> where T : IPermissionable
{
    // коллекция дополнений к правам игрока
    internal List<IPermissionWorker<T>> Workers = new List<IPermissionWorker<T>>();

    // метод который возвращает значение, имеет ли игрок право, сравнивая его между расширениями IPermissionWorker<T>
    internal PermissionAccess HandleResult(Func<IPermissionWorker<T>, PermissionAccess> invokeFunc)
    {
        PermissionAccess access = PermissionAccess.None;
        foreach (var worker in Workers)
        {
            var result = invokeFunc(worker);
            if (result == PermissionAccess.HasPermission)
                access = result;

            if (result == PermissionAccess.Blocked)
                return result;
        }

        return access;
    }
}
```