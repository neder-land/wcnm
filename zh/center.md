# CENTER ������ݰ�

## 1. ��������

### �ͻ���

������ | ��;
:- | :-
[```center.list.games```](#centerlisttypes) | ��ȡ��Ϸ���ĵ�ǰ�ṩ����Ϸ ID �б�
[```center.list.rooms```](#centerlistrooms) | ��ȡĳ����Ϸ���൱ǰ�ķ����б�
[```center.create```](#centercreate) | ����ĳ����Ϸ���·���
[```center.join```](#centerjoin) | ����ĳ����Ϸ��ĳ������
[```center.leave```](#centerleave) | �˳���ǰ����

### �����

������ | ��;
:- | :-
[```center.list.games.response```](#centerlisttypesresponse) | ����Ϸ�����������Ӧ
[```center.list.rooms.response```](#centerlistroomsresponse) | ����Ϸ��������Ļ�Ӧ
[```center.room.change```](#centerroomchange) | ���ڸ�֪�ͻ��˵�ǰ����ı仯������û�б仯�������յ��ͻ��˷��͵�```center.create```��```center.join```��```center.leave```ʱ��������Ӧ�����ش˰�

## 2. Content �淶

### ```center.list.games```

```json5
{
    "action": "center.list.games",
    "content": {}
}
```

### ```center.list.rooms```

```json5
{
    "action": "center.list.rooms",
    "content": {
        "game_type": "" // �������Ϸ���ࣨ��һ������Ϸ ID�������ַ����Ի��ȫ������
    }
}
```

### ```center.create```

```json5
{
    "action": "center.create",
    "content": {
        "room_name": "university", // Ҫ�����ķ��������
        "game": "nswc" // Ҫ��������Ϸ ID
    }
}
```

### ```center.join```

```json5
{
    "action": "center.join",
    "content": {
        "room_name": "university" // Ҫ�������Ϸ������
    }
}
```

### ```center.leave```

```json5
{
    "action": "center.leave",
    "content": {}
}
```

### ```center.list.games.response```

```json5
{
    "action": "center.list.games.response",
    "content": {
        "games": [
            "nswc",
            "ngword",
            "sftql"
        ]
    }
}
```

### ```center.list.rooms.response```

```json5
{
    "action": "center.list.rooms.response",
    "content": {
        "rooms": [
            {
                "name": "university", // ��������
                "game": "nswc", // ��Ϸ ID
				"extra": {
					// �����߼�ʵ�ֵ���չ���ݣ���Ϊ��
				}
            }
        ]
    }
}
```

### ```center.room.change```

Ϊ�������ָ�ʽ֮һ

```json5
{
    "action": "center.room.change",
    "content": {
        "room": {
            "name": "university", // ��������
            "game": "nswc", // ��Ϸ ID
			"extra": {
				// �����߼�ʵ�ֵ���չ���ݣ���Ϊ��
			}
        }
    }
}
```

```json5
{
    "action": "center.room.change",
    "content": {
        "room": null // ֪ͨ�ͻ��˴�ʱ�Ѳ��ڷ�����
    }
}
```