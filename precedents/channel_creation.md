# Управление каналами

## Описание прецедента

**Действующие лица:**
- **Пользователь (Создатель канала):** Инициирует создание и управление каналом.
- **Администратор канала:** Установлен создателем канала; управляет настройками и участниками.
- **Модератор канала:** Назначается администратором или создателем; управляет содержанием канала.
- **Участник:** Подписан на канал и может просматривать публикации.

---

**Цель:**
Предоставить пользователям возможность создавать каналы для общения, информации или сообщества, с гибкими настройками управления и модерации.

---

## Процедуры:

**Создание канала с выбором параметров:**
1. **Пользователь** инициирует создание канала через интерфейс приложения.
2. Вводится основная информация: название канала, тип канала (публичный/приватный), категория (например, "Новости", "Образование").
3. Выбираются настройки: разрешения для участников (только просмотр/комментирование/публикация), возможность подписки по ссылке или только по приглашению, подтверждение настроек и создание канала.
4. Канал добавляется в список каналов **пользователя**.

---

**Установка аватарки канала:**
1. **Создатель** или **администратор** открывает настройки канала.
2. Выбирается опция "Изменить аватарку".
3. Загружается изображение с устройства или выбирается из галереи.
4. Устанавливаются параметры обрезки или оформления изображения (по желанию).
5. Аватарка сохраняется и отображается в канале.

---

**Назначение администраторов и модераторов:**
1. **Создатель** или **администратор** канала открывает список участников.
2. Выбирается участник для назначения роли.
3. Устанавливается роль:
- **Администратор:** имеет доступ к полному управлению каналом.
- **Модератор:** может управлять контентом и комментарием, но не имеет доступа к критическим настройкам.
- **Пользователь** получает уведомление о назначении роли.

*Роль может быть изменена или отозвана через тот же интерфейс.*

---

## Код

```csharp
using System;
using System.Collections.Generic;

public class ChannelManagement
{
    // Хранение информации о каналах
    private List<Channel> channels = new List<Channel>();

    public class Channel
    {
        public string Name { get; set; }
        public string Type { get; set; } // "Public" или "Private"
        public string Category { get; set; }
        public string Avatar { get; set; } = "DefaultAvatar.png";
        public List<User> Admins { get; set; } = new List<User>();
        public List<User> Moderators { get; set; } = new List<User>();
        public List<User> Members { get; set; } = new List<User>();
    }

    public class User
    {
        public string Name { get; set; }
    }

    // Метод для создания канала
    public void CreateChannel(string name, string type, string category, User creator)
    {
        var newChannel = new Channel
        {
            Name = name,
            Type = type,
            Category = category
        };

        // Создатель становится администратором канала
        newChannel.Admins.Add(creator);
        channels.Add(newChannel);

        Console.WriteLine($"Канал '{name}' создан. Тип: {type}, Категория: {category}. Администратор: {creator.Name}.");
    }

    // Метод для установки аватарки канала
    public void SetChannelAvatar(string channelName, string avatarPath)
    {
        var channel = FindChannel(channelName);
        if (channel == null)
        {
            Console.WriteLine($"Канал '{channelName}' не найден.");
            return;
        }

        channel.Avatar = avatarPath;
        Console.WriteLine($"Для канала '{channelName}' установлена аватарка: {avatarPath}.");
    }

    // Метод для назначения администраторов и модераторов
    public void AssignRole(string channelName, User user, string role)
    {
        var channel = FindChannel(channelName);
        if (channel == null)
        {
            Console.WriteLine($"Канал '{channelName}' не найден.");
            return;
        }

        switch (role.ToLower())
        {
            case "admin":
                if (!channel.Admins.Contains(user))
                {
                    channel.Admins.Add(user);
                    Console.WriteLine($"Пользователь {user.Name} назначен администратором канала '{channelName}'.");
                }
                break;

            case "moderator":
                if (!channel.Moderators.Contains(user))
                {
                    channel.Moderators.Add(user);
                    Console.WriteLine($"Пользователь {user.Name} назначен модератором канала '{channelName}'.");
                }
                break;

            default:
                Console.WriteLine($"Роль '{role}' не распознана. Используйте 'admin' или 'moderator'.");
                break;
        }
    }

    // Метод для поиска канала по имени
    private Channel FindChannel(string channelName)
    {
        return channels.Find(c => c.Name.Equals(channelName, StringComparison.OrdinalIgnoreCase));
    }

    // Демонстрация работы
    public static void Main(string[] args)
    {
        var manager = new ChannelManagement();

        // Создание пользователей
        var user1 = new User { Name = "Alice" };
        var user2 = new User { Name = "Bob" };

        // Создание канала
        manager.CreateChannel("TechTalks", "Public", "Education", user1);

        // Установка аватарки
        manager.SetChannelAvatar("TechTalks", "TechAvatar.png");

        // Назначение ролей
        manager.AssignRole("TechTalks", user2, "moderator");
    }
}
---
```
