# l10n_es
Localization seed files for [OFN](https://github.com/openfoodfoundation) ES installation. 

---------------------------------------------------------------

## Data Structure

**states.yml** --> Provincias

```yaml
    ---
    - name: A Coruña (La Coruña)
      country_id: '188'
      id: '101'
      abbr: C
    - name: Araba (Álaba)
      country_id: '188'
      id: '102'
      abbr: VI
```

**countries.yml** --> Only need ES

```yaml
    ---
    countries_188:
      id: '188'
      name: Spain
      iso3: ESP
      iso: ES
      iso_name: SPAIN
      numcode: '724'
```

## How it works?

### provision.yml
When you run `playbooks/provisioning.yml` playbook the `app` role run two tasks related with this repository:

[**copy seeds.rb**](https://github.com/openfoodfoundation/ofn-install/blob/538a3004f8cc60671a40310f27f54c7c5445f10a/roles/app/tasks/main.yml#L24)

Copy `seeds.rb` file in `{{ app_path }}/shared/config`. This file loads `states.yml` and creates a record in the db with `name`, abbr and country fields.

[**get l10n repo**](https://github.com/openfoodfoundation/ofn-install/blob/538a3004f8cc60671a40310f27f54c7c5445f10a/roles/app/tasks/main.yml#L55)

You need to specify where is l10n repo located in your `inventory/group_vars` file. In our case is `inventory/group_vars/es.yml`. With the key `l10n_repo` like [here](https://github.com/openfoodfoundation/ofn-install/blob/538a3004f8cc60671a40310f27f54c7c5445f10a/inventory/group_vars/australia.yml#L11).
The task clone l10n repo in `{{ l10n_path }}`.

### deploy.yml
When you run `playbook/deploy.yml` playbook the `deploy` role run two tasks:

[**symlink into the repo**](https://github.com/openfoodfoundation/ofn-install/blob/538a3004f8cc60671a40310f27f54c7c5445f10a/roles/deploy/tasks/main.yml#L55)

Create a symbolic link in `{{ build_path }}/db/default/spree/states.yml` from `{{ l10n_path }}/states.yml` .

[**seed database**](https://github.com/openfoodfoundation/ofn-install/blob/538a3004f8cc60671a40310f27f54c7c5445f10a/roles/deploy/tasks/main.yml#L189)

Run the `rake db:seed`
