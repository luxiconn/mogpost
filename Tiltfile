# -*- mode: Python -*-

load('ext://restart_process', 'docker_build_with_restart')

local_resource(
    'abilities-build',
    'dotnet publish ./services/abilities-service/MogPost.Abilities.csproj -c Release -o out/abilities-service',
    deps = ['services/abilities-service'], ignore = ['services/abilities-service/obj'],
)

docker_build_with_restart(
    'registry.digitalocean.com/mogpost/abilities-service',
    'out/abilities-service',
    entrypoint=['dotnet', 'MogPost.Abilities.dll'],
    dockerfile='services/abilities-service/Dockerfile',
    live_update=[
        sync('out/abilities-service', '/app/out'),
    ],
)

k8s_yaml('services/abilities-service/kubernetes.yaml')
k8s_resource('abilities-service', port_forwards='8080:80', resource_deps=['abilities-build'])


local_resource(
    'rotation-builder-build',
    'dotnet publish ./services/rotation-builder-service/MogPost.RotationBuilder.csproj -c Release -o out/rotation-builder-service',
    deps = ['services/rotation-builder-service'], ignore = ['services/rotation-builder-service/obj'],
)

docker_build_with_restart(
    'registry.digitalocean.com/mogpost/rotation-builder-service',
    'out/rotation-builder-service',
    entrypoint=['dotnet', 'MogPost.RotationBuilder.dll'],
    dockerfile='services/rotation-builder-service/Dockerfile',
    live_update=[
        sync('out/rotation-builder-service', '/app/out'),
    ],
)

k8s_yaml('services/rotation-builder-service/kubernetes.yaml')
k8s_resource('rotation-builder-service', port_forwards='8081:80', resource_deps=['rotation-builder-build'])