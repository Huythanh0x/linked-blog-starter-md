### https://app.famly.co/graphql?FrameworksForObservations

```sh
curl 'https://app.famly.co/graphql?ObservationsByIds' \
  -H 'accept: */*' \
  -H 'accept-language: en-US,en;q=0.9' \
  -H 'baggage: sentry-environment=production,sentry-release=3672fcbe2c,sentry-public_key=db8a16bb638f4264985f17bb9507c208,sentry-trace_id=e5ec50f4540a425a999815c44ecec455' \
  -H 'content-type: application/json' \
  -b 'famly.session-marker=3434e275-1e69-49e1-8918-75654a267b49; __stripe_mid=b029d495-3021-4444-a89a-1278977ca022c3724f; __stripe_sid=8414c7a3-9b9a-408f-bace-2b6ca53135f789847b' \
  -H 'origin: https://app.famly.co' \
  -H 'priority: u=1, i' \
  -H 'referer: https://app.famly.co/' \
  -H 'sec-ch-ua: "Not;A=Brand";v="99", "Brave";v="139", "Chromium";v="139"' \
  -H 'sec-ch-ua-mobile: ?0' \
  -H 'sec-ch-ua-platform: "macOS"' \
  -H 'sec-fetch-dest: empty' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-site: same-origin' \
  -H 'sec-gpc: 1' \
  -H 'sentry-trace: e5ec50f4540a425a999815c44ecec455-a8c3b9b140302081' \
  -H 'user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/139.0.0.0 Safari/537.36' \
  -H 'x-famly-accesstoken: 81a459ab-df56-40a0-ab74-09adc5c175c4' \
  -H 'x-famly-installationid: 1171fb0c-2f7d-44fe-b8dc-d049073d25d8' \
  -H 'x-famly-platform: html' \
  -H 'x-famly-request-uuid: 1a1e5a09-9538-4c21-a0fe-820facc17379' \
  -H 'x-famly-route: /account/feed/me' \
  -H 'x-famly-version: 3672fcbe2c' \
  --data-raw $'{"operationName":"ObservationsByIds","variables":{"observationIds":["7015c9de-93ee-440a-83b5-e4d5b9c4ca41"]},"query":"query ObservationsByIds($observationIds: [ObservationId\u0021]\u0021) {\\n  childDevelopment {\\n    observations(first: 100, observationIds: $observationIds, ignoreMissing: true) {\\n      results {\\n        ...ObservationData\\n        __typename\\n      }\\n      __typename\\n    }\\n    __typename\\n  }\\n}\\n\\nfragment ObservationData on Observation {\\n  ...ObservationDataWithNoComments\\n  comments {\\n    count\\n    results {\\n      ...Comment\\n      __typename\\n    }\\n    __typename\\n  }\\n  __typename\\n}\\n\\nfragment Comment on Comment {\\n  behaviors {\\n    id: behaviorId\\n    __typename\\n  }\\n  body\\n  id\\n  likes {\\n    count\\n    likedByMe\\n    likes {\\n      ...Like\\n      __typename\\n    }\\n    __typename\\n  }\\n  sentAt\\n  sentBy {\\n    name {\\n      fullName\\n      __typename\\n    }\\n    profileImage {\\n      url\\n      __typename\\n    }\\n    __typename\\n  }\\n  canReport\\n  __typename\\n}\\n\\nfragment Like on Like {\\n  likedAt\\n  reaction\\n  likedBy {\\n    profileImage {\\n      url\\n      __typename\\n    }\\n    name {\\n      firstName\\n      fullName\\n      __typename\\n    }\\n    __typename\\n  }\\n  __typename\\n}\\n\\nfragment ObservationDataWithNoComments on Observation {\\n  children {\\n    id\\n    name\\n    institutionId\\n    profileImage {\\n      url\\n      __typename\\n    }\\n    __typename\\n  }\\n  id\\n  version\\n  feedItem {\\n    id\\n    __typename\\n  }\\n  createdBy {\\n    name {\\n      fullName\\n      __typename\\n    }\\n    profileImage {\\n      url\\n      __typename\\n    }\\n    __typename\\n  }\\n  status {\\n    state\\n    createdAt\\n    __typename\\n  }\\n  variant\\n  settings {\\n    assessmentSetting {\\n      assessmentSettingsId\\n      title\\n      __typename\\n    }\\n    __typename\\n  }\\n  behaviors {\\n    id: behaviorId\\n    ... on BehaviorCanLinkToFrameworks {\\n      ...BehaviorCanLinkToFrameworks\\n      __typename\\n    }\\n    ... on BehaviorObservationVariantAmbiguity {\\n      variants\\n      __typename\\n    }\\n    __typename\\n  }\\n  remark {\\n    id\\n    body\\n    richTextBody\\n    date\\n    statements {\\n      refinement\\n      statement {\\n        body\\n        id\\n        area {\\n          frameworkId\\n          id\\n          lower\\n          upper\\n          title\\n          abbr\\n          color\\n          deletedAt\\n          subAreas {\\n            title\\n            __typename\\n          }\\n          __typename\\n        }\\n        __typename\\n      }\\n      __typename\\n    }\\n    areas {\\n      area {\\n        frameworkId\\n        id\\n        parentId\\n        title\\n        description\\n        abbr\\n        color\\n        placement\\n        deletedAt\\n        framework {\\n          id\\n          title\\n          owner\\n          __typename\\n        }\\n        __typename\\n      }\\n      refinement\\n      note\\n      areaRefinementSettings {\\n        ageBandSetting {\\n          ...AgeBandSetting\\n          __typename\\n        }\\n        assessmentOptionSetting {\\n          ...AssessmentOptionSetting\\n          __typename\\n        }\\n        __typename\\n      }\\n      __typename\\n    }\\n    customFieldValues {\\n      customFieldSetting {\\n        assessmentSettingsId\\n        customFieldId\\n        label\\n        order\\n        __typename\\n      }\\n      value\\n      __typename\\n    }\\n    __typename\\n  }\\n  nextStep {\\n    id\\n    body\\n    richTextBody\\n    __typename\\n  }\\n  files {\\n    name\\n    url\\n    id\\n    __typename\\n  }\\n  images {\\n    height\\n    width\\n    id\\n    secret {\\n      crop\\n      expires\\n      key\\n      path\\n      prefix\\n      __typename\\n    }\\n    __typename\\n  }\\n  videos {\\n    ... on TranscodingVideo {\\n      id\\n      __typename\\n    }\\n    ... on TranscodedVideo {\\n      duration\\n      height\\n      id\\n      thumbnailUrl\\n      videoUrl\\n      width\\n      __typename\\n    }\\n    __typename\\n  }\\n  likes {\\n    count\\n    likedByMe\\n    reactedByMe\\n    likes {\\n      ...Like\\n      __typename\\n    }\\n    __typename\\n  }\\n  canReport\\n  activities {\\n    ...ObservationActivity\\n    __typename\\n  }\\n  __typename\\n}\\n\\nfragment BehaviorCanLinkToFrameworks on BehaviorCanLinkToFrameworks {\\n  id: behaviorId\\n  __typename\\n  frameworks {\\n    ...MinimalFramework\\n    __typename\\n  }\\n}\\n\\nfragment MinimalFramework on Framework {\\n  id\\n  title\\n  abbr\\n  childDevelopmentVersion\\n  areas {\\n    title\\n    description\\n    abbr\\n    color\\n    id\\n    __typename\\n  }\\n  __typename\\n}\\n\\nfragment AgeBandSetting on AgeBandSetting {\\n  ageBandSettingId\\n  id: ageBandSettingId\\n  assessmentSettingsId\\n  from\\n  to\\n  label\\n  __typename\\n}\\n\\nfragment AssessmentOptionSetting on AssessmentOptionSetting {\\n  assessmentOptionSettingId\\n  id: assessmentOptionSettingId\\n  assessmentSettingsId\\n  backgroundColor\\n  fontColor\\n  label\\n  __typename\\n}\\n\\nfragment ObservationActivity on ObservationActivity {\\n  order\\n  activity {\\n    latest {\\n      activityId\\n      images {\\n        url\\n        __typename\\n      }\\n      title\\n      description\\n      __typename\\n    }\\n    __typename\\n  }\\n  __typename\\n}"}'
```

### https://app.famly.co/api/feed/feed/feed?heightTarget=1006
```sh
curl 'https://app.famly.co/api/feed/feed/feed?heightTarget=1006' \
  -H 'accept: */*' \
  -H 'accept-language: en-US,en;q=0.9' \
  -H 'baggage: sentry-environment=production,sentry-release=3672fcbe2c,sentry-public_key=db8a16bb638f4264985f17bb9507c208,sentry-trace_id=e5ec50f4540a425a999815c44ecec455,sentry-sample_rate=0.01,sentry-transaction=%2Faccount%2Ffeed%2Fme,sentry-sampled=false' \
  -H 'content-type: application/json' \
  -b 'famly.session-marker=3434e275-1e69-49e1-8918-75654a267b49' \
  -H 'priority: u=1, i' \
  -H 'referer: https://app.famly.co/' \
  -H 'sec-ch-ua: "Not;A=Brand";v="99", "Brave";v="139", "Chromium";v="139"' \
  -H 'sec-ch-ua-mobile: ?0' \
  -H 'sec-ch-ua-platform: "macOS"' \
  -H 'sec-fetch-dest: empty' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-site: same-origin' \
  -H 'sec-gpc: 1' \
  -H 'sentry-trace: e5ec50f4540a425a999815c44ecec455-b2efdd4af7bc9c75-0' \
  -H 'user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/139.0.0.0 Safari/537.36' \
  -H 'x-famly-accesstoken: 81a459ab-df56-40a0-ab74-09adc5c175c4' \
  -H 'x-famly-installationid: 1171fb0c-2f7d-44fe-b8dc-d049073d25d8' \
  -H 'x-famly-platform: html' \
  -H 'x-famly-request-uuid: 6e8a2d74-0c0c-420e-acf7-e92dca82cc5c' \
  -H 'x-famly-route: /account/feed/me' \
  -H 'x-famly-version: 3672fcbe2c'
```

```sh
curl 'https://app.famly.co/api/v2/images/tagged?childId=d440de72-802f-4f95-8983-2f62f038fbc0&limit=3000' \
  -H 'accept: */*' \
  -H 'accept-language: en-US,en;q=0.6' \
  -H 'baggage: sentry-environment=production,sentry-release=434a48c86a,sentry-public_key=db8a16bb638f4264985f17bb9507c208,sentry-trace_id=df4f85d1c2324e159ef06b70aae99a8a' \
  -H 'content-type: application/json' \
  -b 'intercom-id-fazkk12b=40610f69-889f-4d55-9c7e-8588834db547; intercom-session-fazkk12b=; intercom-device-id-fazkk12b=d55066d8-52f8-4795-80e8-55a630fbde72; __stripe_mid=3409f271-de05-4c24-9ec0-d642e75ecbf89e0e2f; __stripe_sid=47938c13-bd4a-4b0e-9ffd-8c8b933ae2a94dca64; famly.session-marker=08365979-00af-473e-9675-7632e7cac6da' \
  -H 'priority: u=1, i' \
  -H 'referer: https://app.famly.co/' \
  -H 'sec-ch-ua: "Not)A;Brand";v="8", "Chromium";v="138", "Brave";v="138"' \
  -H 'sec-ch-ua-mobile: ?0' \
  -H 'sec-ch-ua-platform: "macOS"' \
  -H 'sec-fetch-dest: empty' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-site: same-origin' \
  -H 'sec-gpc: 1' \
  -H 'sentry-trace: df4f85d1c2324e159ef06b70aae99a8a-b2472f2e81c5c295' \
  -H 'user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/138.0.0.0 Safari/537.36' \
  -H 'x-famly-accesstoken: 722cc5c2-69a4-47d8-8293-c8ce4ff445a5' \
  -H 'x-famly-installationid: 56441cea-2bde-4733-b954-6f288ee36056' \
  -H 'x-famly-platform: html' \
  -H 'x-famly-request-uuid: 60b8ee2d-27b5-4d4d-8127-b42a561d9e44' \
  -H 'x-famly-route: /account/childProfile/:childId/activity' \
  -H 'x-famly-version: 434a48c86a'
```