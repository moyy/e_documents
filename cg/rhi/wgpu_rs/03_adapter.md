[TOC]

# Adapter

|方法名|作用|说明|
|--|--|--|
|is_surface_supported|是否 支持 传进去的 Surface||
|features = get_texture_format_features|支持的 纹理格式|如 features.allowed_usages.is_empty() 返回 true，则 该 格式 不可用|
|get_downlevel_capabilities|Backend 扩展功能||