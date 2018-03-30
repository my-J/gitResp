

	@Override
	public void configPlugin(Plugins me) {

		// 配置DruidPlugin插件
		DruidPlugin druidPlugin = new DruidPlugin(PropKit.get("jdbcUrl"),
				PropKit.get("user"), PropKit.get("password").trim());
		me.add(druidPlugin);

		// 配置ActiveRecord
		ActiveRecordPlugin arp = new ActiveRecordPlugin(druidPlugin);
		arp.setBaseSqlTemplatePath(PathKit.getRootClassPath());
		arp.addSqlTemplate("sql/learing.sql");

		// 设置数据库方言
		arp.setDialect(new AnsiSqlDialect());
		arp.setShowSql(true);

		_MappingKit.mapping(arp);
		me.add(arp);
	}

	@Override
	public void configInterceptor(Interceptors me) {

	}

	@Override
	public void configHandler(Handlers me) {

	}

	public static void main(String[] args) {
		JFinal.start("src/main/webapp/", 8080, "/", 5);
	}
	nskcvjnskabvbvjkabjk

}
