-- 1. ProductTemplate
CREATE TABLE product_templates (
  id SERIAL PRIMARY KEY,
  template_str_id VARCHAR(50) UNIQUE NOT NULL,
  name VARCHAR(100) NOT NULL,
  base_price NUMERIC DEFAULT 0
);

-- 2. OptionCategory
CREATE TABLE option_categories (
  id SERIAL PRIMARY KEY,
  category_str_id VARCHAR(50) UNIQUE NOT NULL,
  name VARCHAR(100) NOT NULL,
  product_template_id INT REFERENCES product_templates(id) ON DELETE CASCADE
);

-- 3. OptionChoice
CREATE TABLE option_choices (
  id SERIAL PRIMARY KEY,
  choice_str_id VARCHAR(50) UNIQUE NOT NULL,
  name VARCHAR(100) NOT NULL,
  price_delta NUMERIC DEFAULT 0,
  option_category_id INT REFERENCES option_categories(id) ON DELETE CASCADE
);

-- 4. CompatibilityRule
CREATE TABLE compatibility_rules (
  id SERIAL PRIMARY KEY,
  rule_type VARCHAR(20) CHECK (rule_type IN ('REQUIRES', 'INCOMPATIBLE_WITH')),
  product_template_id INT REFERENCES product_templates(id) ON DELETE CASCADE,
  primary_choice_id INT REFERENCES option_choices(id),
  secondary_choice_id INT REFERENCES option_choices(id)
);
