## Aula 29 - Listando notificações do usuário

- Criar uma rota notifications
- Criar o controller `NotificationController.js`:

```
import Notification from '../schemas/Notification';
import User from '../models/User';

class NotificationController {
  async index(req, res) {
    /**
     * Check if loggedUser is a provider
     */
    const checkIsProvider = await User.findOne({
      where: {
        id: req.userId,
        provider: true,
      },
    });

    if (!checkIsProvider) {
      return res
        .status(401)
        .json({ error: 'Only provider can load notifications' });
    }

    const notifications = await Notification.find({
      user: req.userId,
    })
      .sort({ createdAt: 'desc' })
      .limit(20);

    return res.json(notifications);
  }
}

export default new NotificationController();
```


Fim: [https://github.com/tgmarinho/gobarber/tree/aula29](https://github.com/tgmarinho/gobarber/tree/aula29)
