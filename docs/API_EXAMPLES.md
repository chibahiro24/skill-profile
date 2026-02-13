# API データ形式例

このドキュメントでは、Skill Profile APIで使用されるデータ形式の例を示します。

## 目次

- [認証](#認証)
- [ユーザー](#ユーザー)
- [スキル](#スキル)
- [プロジェクト](#プロジェクト)
- [活動ログ](#活動ログ)
- [検出されたスキル](#検出されたスキル)
- [プラグイン](#プラグイン)
- [アクセス制御](#アクセス制御)
- [エラーレスポンス](#エラーレスポンス)

---

## 認証

### POST /api/v1/auth/login

**リクエスト:**
```json
{
  "email": "user@example.com",
  "password": "SecurePassword123!"
}
```

**レスポンス (200 OK):**
```json
{
  "success": true,
  "data": {
    "user": {
      "id": "usr_123abc",
      "email": "user@example.com",
      "displayName": "田中太郎",
      "avatarUrl": "https://cdn.skillprofile.com/avatars/usr_123abc.jpg"
    },
    "tokens": {
      "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
      "refreshToken": "rt_abc123xyz...",
      "expiresIn": 604800,
      "tokenType": "Bearer"
    }
  }
}
```

### POST /api/v1/auth/register

**リクエスト:**
```json
{
  "email": "newuser@example.com",
  "password": "SecurePassword123!",
  "displayName": "山田花子",
  "acceptTerms": true
}
```

**レスポンス (201 Created):**
```json
{
  "success": true,
  "data": {
    "user": {
      "id": "usr_456def",
      "email": "newuser@example.com",
      "displayName": "山田花子",
      "avatarUrl": null,
      "createdAt": "2026-02-13T10:30:00Z"
    },
    "message": "確認メールを送信しました。メールを確認してアカウントを有効化してください。"
  }
}
```

---

## ユーザー

### GET /api/v1/users/me

**レスポンス (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "usr_123abc",
    "email": "user@example.com",
    "displayName": "田中太郎",
    "avatarUrl": "https://cdn.skillprofile.com/avatars/usr_123abc.jpg",
    "bio": "フルスタックエンジニア。Web開発とAIに興味があります。",
    "location": "Tokyo, Japan",
    "website": "https://tanaka-taro.dev",
    "githubUsername": "tanaka-taro",
    "twitterUsername": "tanaka_dev",
    "linkedinUrl": "https://linkedin.com/in/tanaka-taro",
    "skills": {
      "total": 25,
      "byCategory": {
        "programming": 15,
        "framework": 8,
        "tool": 12
      }
    },
    "stats": {
      "projectsCount": 42,
      "certificatesCount": 8,
      "githubContributions": 1523,
      "profileViews": 328,
      "lastActive": "2026-02-13T15:45:00Z"
    },
    "settings": {
      "isPublic": true,
      "showEmail": false,
      "showLocation": true,
      "allowIndexing": true
    },
    "createdAt": "2024-01-15T09:00:00Z",
    "updatedAt": "2026-02-13T14:20:00Z"
  }
}
```

### GET /api/v1/users/:userId

**レスポンス (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "usr_123abc",
    "displayName": "田中太郎",
    "avatarUrl": "https://cdn.skillprofile.com/avatars/usr_123abc.jpg",
    "bio": "フルスタックエンジニア。Web開発とAIに興味があります。",
    "location": "Tokyo, Japan",
    "website": "https://tanaka-taro.dev",
    "skills": [
      {
        "id": "skl_001",
        "name": "TypeScript",
        "category": "Programming Language",
        "level": 4,
        "experienceYears": 5.5,
        "endorsements": 23
      },
      {
        "id": "skl_002",
        "name": "React",
        "category": "Framework",
        "level": 5,
        "experienceYears": 6,
        "endorsements": 45
      }
    ],
    "topProjects": [
      {
        "id": "prj_001",
        "name": "E-commerce Platform",
        "description": "大規模ECサイトの開発",
        "technologies": ["React", "Node.js", "PostgreSQL"],
        "startDate": "2024-01-01",
        "endDate": "2024-12-31"
      }
    ],
    "stats": {
      "projectsCount": 42,
      "certificatesCount": 8,
      "githubContributions": 1523
    }
  }
}
```

---

## スキル

### GET /api/v1/skills

**クエリパラメータ:**
- `category` (optional): カテゴリでフィルタ
- `level` (optional): レベルでフィルタ (1-5)
- `sort` (optional): ソート順 (`name`, `level`, `experience`, `updated`)
- `limit` (optional): 取得件数 (デフォルト: 50)
- `offset` (optional): オフセット

**レスポンス (200 OK):**
```json
{
  "success": true,
  "data": {
    "skills": [
      {
        "id": "skl_001",
        "name": "TypeScript",
        "category": "Programming Language",
        "level": 4,
        "experienceYears": 5.5,
        "description": "型安全なJavaScriptスーパーセット。大規模アプリケーション開発に使用。",
        "isPublic": true,
        "certificates": [
          {
            "id": "cert_001",
            "name": "TypeScript Advanced Certification",
            "issuer": "Microsoft",
            "issueDate": "2023-06-15",
            "credentialUrl": "https://learn.microsoft.com/credentials/cert_001"
          }
        ],
        "detectedFrom": [
          {
            "source": "github",
            "count": 234,
            "lastDetected": "2026-02-13T10:00:00Z"
          },
          {
            "source": "chatgpt",
            "count": 45,
            "lastDetected": "2026-02-12T15:30:00Z"
          }
        ],
        "relatedSkills": ["JavaScript", "React", "Node.js"],
        "endorsements": 23,
        "createdAt": "2024-03-20T09:00:00Z",
        "updatedAt": "2026-02-13T14:20:00Z"
      },
      {
        "id": "skl_002",
        "name": "React",
        "category": "Framework",
        "level": 5,
        "experienceYears": 6,
        "description": "モダンなUIライブラリ。複雑なSPAの構築に精通。",
        "isPublic": true,
        "certificates": [],
        "detectedFrom": [
          {
            "source": "vscode",
            "count": 567,
            "lastDetected": "2026-02-13T16:00:00Z"
          }
        ],
        "relatedSkills": ["TypeScript", "Next.js", "Redux"],
        "endorsements": 45,
        "createdAt": "2024-02-10T10:00:00Z",
        "updatedAt": "2026-02-13T16:00:00Z"
      }
    ],
    "pagination": {
      "total": 25,
      "limit": 50,
      "offset": 0,
      "hasMore": false
    },
    "aggregations": {
      "byCategory": {
        "Programming Language": 8,
        "Framework": 7,
        "Database": 4,
        "Tool": 6
      },
      "byLevel": {
        "1": 2,
        "2": 5,
        "3": 8,
        "4": 7,
        "5": 3
      }
    }
  }
}
```

### POST /api/v1/skills

**リクエスト:**
```json
{
  "name": "Python",
  "category": "Programming Language",
  "level": 3,
  "experienceYears": 2.5,
  "description": "データ分析とバックエンド開発に使用",
  "isPublic": true
}
```

**レスポンス (201 Created):**
```json
{
  "success": true,
  "data": {
    "id": "skl_003",
    "name": "Python",
    "category": "Programming Language",
    "level": 3,
    "experienceYears": 2.5,
    "description": "データ分析とバックエンド開発に使用",
    "isPublic": true,
    "certificates": [],
    "detectedFrom": [],
    "relatedSkills": [],
    "endorsements": 0,
    "createdAt": "2026-02-13T17:00:00Z",
    "updatedAt": "2026-02-13T17:00:00Z"
  }
}
```

---

## プロジェクト

### GET /api/v1/projects

**レスポンス (200 OK):**
```json
{
  "success": true,
  "data": {
    "projects": [
      {
        "id": "prj_001",
        "name": "E-commerce Platform",
        "description": "大規模ECサイトのフルスタック開発",
        "role": "テックリード",
        "startDate": "2024-01-01",
        "endDate": "2024-12-31",
        "status": "completed",
        "teamSize": 12,
        "technologies": [
          {
            "id": "skl_002",
            "name": "React",
            "category": "Framework"
          },
          {
            "id": "skl_004",
            "name": "Node.js",
            "category": "Runtime"
          },
          {
            "id": "skl_005",
            "name": "PostgreSQL",
            "category": "Database"
          }
        ],
        "achievements": [
          "月間100万PVを達成",
          "レスポンスタイムを50%改善",
          "コンバージョン率を30%向上"
        ],
        "urls": {
          "website": "https://example-ecommerce.com",
          "github": "https://github.com/company/ecommerce-platform"
        },
        "isPublic": true,
        "createdAt": "2024-01-05T10:00:00Z",
        "updatedAt": "2025-01-10T14:00:00Z"
      }
    ],
    "pagination": {
      "total": 42,
      "limit": 20,
      "offset": 0,
      "hasMore": true
    }
  }
}
```

---

## 活動ログ

### GET /api/v1/activity-logs

**クエリパラメータ:**
- `sourceType` (optional): ソースタイプ (`ai_conversation`, `code_edit`, `github_activity`, `learning`)
- `pluginId` (optional): プラグインIDでフィルタ
- `startDate` (optional): 開始日時
- `endDate` (optional): 終了日時
- `processed` (optional): 処理済みかどうか (`true`, `false`)

**レスポンス (200 OK):**
```json
{
  "success": true,
  "data": {
    "logs": [
      {
        "id": "log_001",
        "userId": "usr_123abc",
        "pluginId": "plg_chatgpt",
        "pluginName": "ChatGPT Plugin",
        "sourceType": "ai_conversation",
        "metadata": {
          "conversationId": "conv_xyz123",
          "model": "gpt-4",
          "language": "ja",
          "topics": ["React", "TypeScript", "Performance Optimization"],
          "messageCount": 12,
          "duration": 1800
        },
        "contentPreview": "Reactのパフォーマンス最適化について...",
        "wordCount": 856,
        "detectedTopics": ["React", "useMemo", "React.memo", "パフォーマンス"],
        "processed": true,
        "processedAt": "2026-02-13T11:00:00Z",
        "skillsDetected": 5,
        "createdAt": "2026-02-13T10:30:00Z"
      },
      {
        "id": "log_002",
        "userId": "usr_123abc",
        "pluginId": "plg_github",
        "pluginName": "GitHub Plugin",
        "sourceType": "github_activity",
        "metadata": {
          "repository": "company/project-x",
          "commitSha": "abc123def456",
          "branch": "feature/new-api",
          "additions": 234,
          "deletions": 56,
          "filesChanged": 8,
          "languages": ["TypeScript", "SQL"]
        },
        "contentPreview": "feat: Add new API endpoint for user management...",
        "wordCount": 45,
        "detectedTopics": ["API", "TypeScript", "PostgreSQL"],
        "processed": true,
        "processedAt": "2026-02-13T09:30:00Z",
        "skillsDetected": 3,
        "createdAt": "2026-02-13T09:00:00Z"
      }
    ],
    "pagination": {
      "total": 1523,
      "limit": 20,
      "offset": 0,
      "hasMore": true
    },
    "stats": {
      "totalLogs": 1523,
      "processedLogs": 1498,
      "pendingLogs": 25,
      "bySourceType": {
        "ai_conversation": 456,
        "code_edit": 892,
        "github_activity": 145,
        "learning": 30
      }
    }
  }
}
```

---

## 検出されたスキル

### GET /api/v1/detected-skills

**クエリパラメータ:**
- `status` (optional): ステータス (`pending`, `approved`, `rejected`)
- `minConfidence` (optional): 最小信頼度 (0-1)
- `pluginId` (optional): プラグインIDでフィルタ

**レスポンス (200 OK):**
```json
{
  "success": true,
  "data": {
    "detectedSkills": [
      {
        "id": "det_001",
        "activityLogId": "log_001",
        "skillName": "React Performance Optimization",
        "skillCategory": "Framework",
        "confidenceScore": 0.87,
        "contextSnippet": "...useMemoとReact.memoを使用してレンダリング回数を削減...",
        "detectionMethod": "nlp",
        "relatedSkills": ["React", "useMemo", "React.memo"],
        "detectedBy": {
          "pluginId": "plg_chatgpt",
          "pluginName": "ChatGPT Plugin"
        },
        "evidence": {
          "sourceType": "ai_conversation",
          "occurrences": 5,
          "keywords": ["useMemo", "React.memo", "最適化", "パフォーマンス"],
          "complexity": "advanced"
        },
        "status": "pending",
        "createdAt": "2026-02-13T11:00:00Z"
      },
      {
        "id": "det_002",
        "activityLogId": "log_002",
        "skillName": "PostgreSQL",
        "skillCategory": "Database",
        "confidenceScore": 0.92,
        "contextSnippet": "...CREATE INDEX ON users (email) WHERE active = true...",
        "detectionMethod": "keyword",
        "relatedSkills": ["SQL", "Database Design", "Indexing"],
        "detectedBy": {
          "pluginId": "plg_github",
          "pluginName": "GitHub Plugin"
        },
        "evidence": {
          "sourceType": "github_activity",
          "occurrences": 12,
          "keywords": ["PostgreSQL", "SQL", "INDEX", "CREATE TABLE"],
          "complexity": "intermediate"
        },
        "status": "approved",
        "userReviewedAt": "2026-02-13T12:00:00Z",
        "mergedToSkillId": "skl_005",
        "createdAt": "2026-02-13T09:30:00Z"
      }
    ],
    "pagination": {
      "total": 45,
      "limit": 20,
      "offset": 0,
      "hasMore": true
    },
    "summary": {
      "pending": 23,
      "approved": 18,
      "rejected": 4,
      "averageConfidence": 0.84,
      "topCategories": [
        { "category": "Framework", "count": 12 },
        { "category": "Programming Language", "count": 8 },
        { "category": "Database", "count": 6 }
      ]
    }
  }
}
```

### POST /api/v1/detected-skills/:id/approve

**リクエスト:**
```json
{
  "mergeWithExisting": true,
  "targetSkillId": "skl_002",
  "adjustments": {
    "level": 4,
    "description": "React hooks とパフォーマンス最適化に精通"
  }
}
```

**レスポンス (200 OK):**
```json
{
  "success": true,
  "data": {
    "detectedSkill": {
      "id": "det_001",
      "status": "approved",
      "mergedToSkillId": "skl_002",
      "userReviewedAt": "2026-02-13T17:30:00Z"
    },
    "updatedSkill": {
      "id": "skl_002",
      "name": "React",
      "level": 4,
      "experienceYears": 6,
      "description": "React hooks とパフォーマンス最適化に精通",
      "updatedAt": "2026-02-13T17:30:00Z"
    }
  }
}
```

---

## プラグイン

### GET /api/v1/plugins

**クエリパラメータ:**
- `category` (optional): カテゴリでフィルタ
- `installed` (optional): インストール済みのみ (`true`, `false`)
- `enabled` (optional): 有効なプラグインのみ (`true`, `false`)

**レスポンス (200 OK):**
```json
{
  "success": true,
  "data": {
    "plugins": [
      {
        "id": "plg_chatgpt",
        "name": "ChatGPT Plugin",
        "version": "1.2.0",
        "author": "Skill Profile Team",
        "description": "ChatGPTとの会話からスキルを自動抽出",
        "icon": "https://cdn.skillprofile.com/plugins/chatgpt/icon.png",
        "categories": ["ai-integration", "automation"],
        "permissions": ["read:logs", "write:skills", "network:external"],
        "rating": {
          "average": 4.7,
          "count": 1234
        },
        "stats": {
          "installCount": 5678,
          "activeUsers": 4521
        },
        "isOfficial": true,
        "status": "published",
        "compatibility": {
          "minVersion": "1.0.0",
          "maxVersion": "2.x.x"
        },
        "urls": {
          "homepage": "https://github.com/skill-profile/chatgpt-plugin",
          "documentation": "https://docs.skillprofile.com/plugins/chatgpt",
          "repository": "https://github.com/skill-profile/chatgpt-plugin"
        },
        "userInstallation": {
          "isInstalled": true,
          "isEnabled": true,
          "installedAt": "2026-01-15T10:00:00Z",
          "lastUsed": "2026-02-13T10:30:00Z",
          "logsCollected": 456,
          "skillsDetected": 78
        }
      },
      {
        "id": "plg_github",
        "name": "GitHub Plugin",
        "version": "2.0.1",
        "author": "Skill Profile Team",
        "description": "GitHub活動からスキルを分析",
        "icon": "https://cdn.skillprofile.com/plugins/github/icon.png",
        "categories": ["code-analysis", "automation"],
        "permissions": ["read:logs", "write:skills", "network:github"],
        "rating": {
          "average": 4.9,
          "count": 2345
        },
        "stats": {
          "installCount": 8901,
          "activeUsers": 7234
        },
        "isOfficial": true,
        "status": "published",
        "compatibility": {
          "minVersion": "1.0.0",
          "maxVersion": "2.x.x"
        },
        "urls": {
          "homepage": "https://github.com/skill-profile/github-plugin",
          "documentation": "https://docs.skillprofile.com/plugins/github",
          "repository": "https://github.com/skill-profile/github-plugin"
        },
        "userInstallation": {
          "isInstalled": true,
          "isEnabled": true,
          "installedAt": "2026-01-10T09:00:00Z",
          "lastUsed": "2026-02-13T09:00:00Z",
          "logsCollected": 892,
          "skillsDetected": 145
        }
      }
    ],
    "pagination": {
      "total": 28,
      "limit": 20,
      "offset": 0,
      "hasMore": true
    }
  }
}
```

### POST /api/v1/plugins/:pluginId/install

**リクエスト:**
```json
{
  "config": {
    "apiKey": "sk-...",
    "syncInterval": 60,
    "categories": ["all"]
  },
  "autoEnable": true
}
```

**レスポンス (201 Created):**
```json
{
  "success": true,
  "data": {
    "installation": {
      "pluginId": "plg_claude",
      "status": "installed",
      "isEnabled": true,
      "installedAt": "2026-02-13T18:00:00Z",
      "config": {
        "syncInterval": 60,
        "categories": ["all"]
      }
    },
    "message": "プラグインが正常にインストールされました"
  }
}
```

---

## アクセス制御

### GET /api/v1/access-grants

**レスポンス (200 OK):**
```json
{
  "success": true,
  "data": {
    "grants": [
      {
        "id": "ag_001",
        "granteeEmail": "hr@company.com",
        "granteeType": "company",
        "granteeName": "株式会社サンプル",
        "scope": "partial",
        "allowedItems": {
          "skills": ["skl_001", "skl_002", "skl_005"],
          "projects": ["prj_001", "prj_003"],
          "certificates": true,
          "contactInfo": false
        },
        "expiryDate": "2026-03-15T23:59:59Z",
        "isActive": true,
        "accessCount": 12,
        "lastAccessedAt": "2026-02-12T14:30:00Z",
        "createdAt": "2026-02-01T10:00:00Z"
      },
      {
        "id": "ag_002",
        "granteeEmail": "recruiter@headhunt.com",
        "granteeType": "individual",
        "granteeName": "佐藤美咲",
        "scope": "full",
        "allowedItems": null,
        "expiryDate": "2026-02-28T23:59:59Z",
        "isActive": true,
        "accessCount": 3,
        "lastAccessedAt": "2026-02-10T09:15:00Z",
        "createdAt": "2026-01-25T15:00:00Z"
      }
    ],
    "pagination": {
      "total": 8,
      "limit": 20,
      "offset": 0,
      "hasMore": false
    }
  }
}
```

### POST /api/v1/access-grants

**リクエスト:**
```json
{
  "granteeEmail": "hr@newcompany.com",
  "granteeType": "company",
  "granteeName": "新規株式会社",
  "scope": "partial",
  "allowedItems": {
    "skills": ["skl_001", "skl_002"],
    "projects": ["prj_001"],
    "certificates": true,
    "contactInfo": false
  },
  "expiryDate": "2026-04-30T23:59:59Z",
  "notifyGrantee": true,
  "message": "採用選考のためプロフィールを共有します"
}
```

**レスポンス (201 Created):**
```json
{
  "success": true,
  "data": {
    "grant": {
      "id": "ag_003",
      "granteeEmail": "hr@newcompany.com",
      "accessUrl": "https://skillprofile.com/view/ag_003?token=xyz789",
      "expiryDate": "2026-04-30T23:59:59Z",
      "isActive": true,
      "createdAt": "2026-02-13T18:30:00Z"
    },
    "message": "アクセス権限が付与され、通知メールが送信されました"
  }
}
```

### GET /api/v1/access-logs

**レスポンス (200 OK):**
```json
{
  "success": true,
  "data": {
    "logs": [
      {
        "id": "al_001",
        "accessGrantId": "ag_001",
        "granteeEmail": "hr@company.com",
        "granteeName": "株式会社サンプル",
        "accessedAt": "2026-02-12T14:30:00Z",
        "ipAddress": "203.0.113.42",
        "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)...",
        "accessedItems": {
          "skills": ["skl_001", "skl_002"],
          "projects": ["prj_001"],
          "certificates": true
        },
        "duration": 420
      }
    ],
    "pagination": {
      "total": 156,
      "limit": 20,
      "offset": 0,
      "hasMore": true
    },
    "stats": {
      "totalAccesses": 156,
      "uniqueAccessors": 8,
      "lastAccess": "2026-02-12T14:30:00Z",
      "topAccessors": [
        { "email": "hr@company.com", "count": 12 },
        { "email": "recruiter@headhunt.com", "count": 3 }
      ]
    }
  }
}
```

---

## エラーレスポンス

### 認証エラー (401 Unauthorized)

```json
{
  "success": false,
  "error": {
    "code": "UNAUTHORIZED",
    "message": "認証が必要です",
    "details": {
      "reason": "invalid_token",
      "description": "提供されたトークンが無効または期限切れです"
    }
  }
}
```

### 権限エラー (403 Forbidden)

```json
{
  "success": false,
  "error": {
    "code": "FORBIDDEN",
    "message": "このリソースへのアクセス権限がありません",
    "details": {
      "requiredPermission": "write:skills",
      "userPermissions": ["read:skills", "read:projects"]
    }
  }
}
```

### リソースが見つからない (404 Not Found)

```json
{
  "success": false,
  "error": {
    "code": "NOT_FOUND",
    "message": "リソースが見つかりません",
    "details": {
      "resource": "skill",
      "id": "skl_999"
    }
  }
}
```

### バリデーションエラー (400 Bad Request)

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "入力データが無効です",
    "details": {
      "errors": [
        {
          "field": "email",
          "message": "有効なメールアドレスを入力してください",
          "code": "invalid_email"
        },
        {
          "field": "password",
          "message": "パスワードは12文字以上である必要があります",
          "code": "password_too_short"
        }
      ]
    }
  }
}
```

### レート制限エラー (429 Too Many Requests)

```json
{
  "success": false,
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "リクエスト数が制限を超えました",
    "details": {
      "limit": 100,
      "remaining": 0,
      "resetAt": "2026-02-13T19:00:00Z",
      "retryAfter": 180
    }
  }
}
```

### サーバーエラー (500 Internal Server Error)

```json
{
  "success": false,
  "error": {
    "code": "INTERNAL_SERVER_ERROR",
    "message": "サーバーエラーが発生しました",
    "details": {
      "requestId": "req_abc123xyz",
      "timestamp": "2026-02-13T18:45:00Z"
    }
  }
}
```

---

## GraphQL API

### エンドポイント

```
POST /api/graphql
```

### クエリ例

#### ユーザープロフィールとスキル

```graphql
query GetUserProfile($userId: ID!) {
  user(id: $userId) {
    id
    displayName
    avatarUrl
    bio
    location
    skills(limit: 10, sort: LEVEL_DESC) {
      id
      name
      category
      level
      experienceYears
      certificates {
        id
        name
        issuer
        issueDate
      }
    }
    projects(limit: 5) {
      id
      name
      description
      technologies {
        name
        category
      }
      startDate
      endDate
    }
    stats {
      projectsCount
      certificatesCount
      skillsCount
      profileViews
    }
  }
}
```

**変数:**
```json
{
  "userId": "usr_123abc"
}
```

**レスポンス:**
```json
{
  "data": {
    "user": {
      "id": "usr_123abc",
      "displayName": "田中太郎",
      "avatarUrl": "https://cdn.skillprofile.com/avatars/usr_123abc.jpg",
      "bio": "フルスタックエンジニア",
      "location": "Tokyo, Japan",
      "skills": [
        {
          "id": "skl_002",
          "name": "React",
          "category": "Framework",
          "level": 5,
          "experienceYears": 6,
          "certificates": []
        }
      ],
      "projects": [...],
      "stats": {
        "projectsCount": 42,
        "certificatesCount": 8,
        "skillsCount": 25,
        "profileViews": 328
      }
    }
  }
}
```

#### 検出されたスキルの取得と承認

```graphql
query GetDetectedSkills {
  detectedSkills(status: PENDING, minConfidence: 0.8) {
    id
    skillName
    skillCategory
    confidenceScore
    contextSnippet
    evidence {
      occurrences
      keywords
      complexity
    }
    detectedBy {
      pluginName
    }
    createdAt
  }
}

mutation ApproveDetectedSkill($id: ID!, $input: ApproveSkillInput!) {
  approveDetectedSkill(id: $id, input: $input) {
    success
    detectedSkill {
      id
      status
      mergedToSkillId
    }
    updatedSkill {
      id
      name
      level
      updatedAt
    }
  }
}
```

---

## ページネーション

すべてのリストAPIは共通のページネーション形式を使用：

```json
{
  "success": true,
  "data": {
    "items": [...],
    "pagination": {
      "total": 156,
      "limit": 20,
      "offset": 0,
      "hasMore": true,
      "nextOffset": 20,
      "prevOffset": null
    }
  }
}
```

**クエリパラメータ:**
- `limit`: 取得件数 (デフォルト: 20, 最大: 100)
- `offset`: オフセット (デフォルト: 0)
- `sort`: ソート順
- `order`: 昇順/降順 (`asc`, `desc`)

---

## レート制限

すべてのAPIリクエストにはレート制限が適用されます：

**レスポンスヘッダー:**
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 87
X-RateLimit-Reset: 1676304000
```

**制限:**
- 認証済みユーザー: 100 req/min
- 未認証: 10 req/min
- 管理者: 1000 req/min

---

## Webhook

イベント発生時にWebhookでリアルタイム通知を受信できます。

### Webhook ペイロード例

#### skill.detected

```json
{
  "event": "skill.detected",
  "timestamp": "2026-02-13T18:00:00Z",
  "data": {
    "detectedSkill": {
      "id": "det_003",
      "skillName": "Docker",
      "confidenceScore": 0.91,
      "status": "pending"
    },
    "user": {
      "id": "usr_123abc",
      "displayName": "田中太郎"
    }
  }
}
```

#### profile.viewed

```json
{
  "event": "profile.viewed",
  "timestamp": "2026-02-13T18:05:00Z",
  "data": {
    "viewer": {
      "email": "hr@company.com",
      "name": "株式会社サンプル"
    },
    "accessGrantId": "ag_001",
    "ipAddress": "203.0.113.42",
    "user": {
      "id": "usr_123abc"
    }
  }
}
```

---

このAPI仕様は開発中のものであり、実装時に変更される可能性があります。最新の情報は公式APIドキュメント（https://docs.skillprofile.com/api）を参照してください。
