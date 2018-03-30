package com.xiaoyoumanman.system;

import com.jfinal.config.Constants;
import com.jfinal.config.Handlers;
import com.jfinal.config.Interceptors;
import com.jfinal.config.JFinalConfig;
import com.jfinal.config.Plugins;
import com.jfinal.config.Routes;
import com.jfinal.core.JFinal;
import com.jfinal.json.FastJsonFactory;
import com.jfinal.kit.PathKit;
import com.jfinal.kit.PropKit;
import com.jfinal.plugin.activerecord.ActiveRecordPlugin;
import com.jfinal.plugin.activerecord.dialect.AnsiSqlDialect;
import com.jfinal.plugin.druid.DruidPlugin;
import com.jfinal.template.Engine;
import com.xiaoyoumanman.system.model._MappingKit;
import com.xiaoyoumanman.system.routes.AdminRoutes;

public class mainConfig extends JFinalConfig {
	@Override
	public void configConstant(Constants me) {
		PropKit.use("a_little_config.txt");
		me.setDevMode(PropKit.getBoolean("devMode", false));
		me.setJsonFactory(new FastJsonFactory());

	}

	@Override
	public void configRoute(Routes me) {
		me.add(new AdminRoutes());

	}

	@Override
	public void configEngine(Engine me) {
		me.setDevMode(true);
		me.getDevMode();
		me.setBaseTemplatePath(PathKit.getWebRootPath() + "/WEB-INF/jfinal");
		me.addSharedFunction("/common/_layout.html");
		me.addSharedFunction("/common/_form_layout.html");
		me.addSharedFunction("/common/_paginate.html");

	}

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

}
